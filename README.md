erDiagram
      %% ============================================================
      %% 实验室管理系统 - 数据库ER图
      %% ============================================================

      %% 耗材与入库/领用的一对多关系
      consumable ||--o{ consumable_inbound : "入库"
      consumable ||--o{ consumable_outbound : "领用"

      %% ============================================================
      %% 耗材主表：存储耗材基本信息与实时库存
      %% ============================================================
      consumable {
          BIGINT id PK "主键ID"
          VARCHAR code "耗材编号（唯一）"
          VARCHAR name "耗材名称"
          VARCHAR spec "规格型号"
          VARCHAR unit "计量单位"
          INT stock "当前库存数量"
          INT min_stock "最低库存预警阈值"
          VARCHAR location "存放位置"
          VARCHAR supplier "供应商"
          TINYINT status "状态（0:禁用 1:启用）"
          VARCHAR remark "备注"
          DATETIME create_time "创建时间"
          DATETIME update_time "更新时间"
      }

      %% ============================================================
      %% 耗材入库表：记录每次入库明细，入库后自动累加库存
      %% ============================================================
      consumable_inbound {
          BIGINT id PK "主键ID"
          BIGINT consumable_id FK "耗材ID"
          VARCHAR consumable_name "耗材名称（冗余）"
          INT quantity "入库数量"
          VARCHAR unit "计量单位"
          DECIMAL price "单价"
          DECIMAL total_amount "总金额"
          VARCHAR supplier "供应商"
          VARCHAR operator_name "操作人"
          VARCHAR remark "备注"
          DATETIME create_time "入库时间"
      }

      %% ============================================================
      %% 耗材领用表：记录每次领用明细，领用后自动扣减库存
      %% ============================================================
      consumable_outbound {
          BIGINT id PK "主键ID"
          BIGINT consumable_id FK "耗材ID"
          VARCHAR consumable_name "耗材名称（冗余）"
          INT quantity "领用数量"
          VARCHAR unit "计量单位"
          VARCHAR purpose "用途说明"
          VARCHAR applicant_name "领用人"
          VARCHAR operator_name "经办人（审批发放人）"
          TINYINT status "状态（0:已归还 1:已领用）"
          VARCHAR remark "备注"
          DATETIME create_time "领用时间"
      }

      %% ============================================================
      %% 实验室表：独立实体，存储实验室基础档案信息
      %% ============================================================
      lab_info {
          BIGINT id PK "主键ID"
          VARCHAR lab_code "实验室编号（唯一）"
          VARCHAR lab_name "实验室名称"
          VARCHAR location "所在位置"
          INT capacity "容纳人数"
          DECIMAL area "面积（平方米）"
          VARCHAR equipment "主要设备"
          TEXT description "实验室描述"
          TINYINT status "状态（0:禁用 1:启用）"
          DATETIME create_time "创建时间"
          DATETIME update_time "更新时间"
      }

      %% ============================================================
      %% 公告表：独立实体，支持置顶排序与逻辑删除
      %% ============================================================
      notice {
          BIGINT id PK "主键ID"
          VARCHAR title "公告标题"
          LONGTEXT content "公告内容（支持富文本HTML）"
          VARCHAR publisher "发布人"
          TINYINT is_top "是否置顶（0:否 1:是）"
          TINYINT is_deleted "逻辑删除（0:正常 1:已删除）"
          DATETIME create_time "创建时间"
          DATETIME update_time "修改时间"
      }
