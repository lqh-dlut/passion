x_positions

含义：各代理（车辆/行人）在不同时间步的位置坐标（2D坐标，如[X, Y]）。

结构：3D Tensor，形状为 (代理数量, 时间步长, 2)。

示例：如 1463.0780, -1195.3733 表示某代理在某一时刻的位置，末尾的 0.0000 表示无效数据（需结合 x_valid_mask 确认）。

x_attr

含义：代理的属性编码，可能包括类型、状态等（如车辆类型、行人、优先级等）。

结构：2D Tensor，每行对应一个代理的3个属性（如 [0, 1, 0]）。

推测：

第一个值可能为代理类型（如 0=普通车，5=特殊车辆）。

第二个值可能为状态（如 1=运动，3=静止）。

第三个值可能为优先级或交互标志（如 0=无，3=高优先级）。

x_angles

含义：代理的朝向角度（弧度制），反映运动方向。

示例：2.7555 约等于 157.8 度（假设为相对于X轴正方向的逆时针角度）。

x_velocity

含义：代理的速度值（单位可能是米/秒），正数表示运动，0.0000 表示静止或无效数据。

x_valid_mask

含义：布尔掩码，标记 x_positions 中哪些时间步的数据有效（True=有效，False=无效）。

lane_positions

含义：车道线的坐标点序列（2D坐标），描述道路结构。

结构：3D Tensor，形状为 (车道线数量, 车道线点数量, 2)。

示例：[1560.0000, -1236.4850] 表示某车道线的一个点坐标。

lane_attr

含义：车道属性，可能包含类型、宽度、限速等。

结构：每行对应一条车道线的3个属性（如 [0.0, 3.3449, 0.0]）。

推测：

第一个值可能为车道类型（0=普通车道，1=交叉口车道）。

第二个值可能为车道宽度（如 3.3449 米）。

第三个值可能为限速或交通规则标志。

is_intersections

含义：标记对应车道线是否位于交叉口内（1.0=是，0.0=否）。

场景元数据
scenario_id：场景唯一标识符（如 0a0af725-fbc3-41de-b969-3be718f694e2）。

agent_ids：代理ID列表，包含普通车辆（如 9024）和自动驾驶车辆（AV）。

focal_idx：焦点代理索引（值为 3，对应 agent_ids 中的 9024）。

scored_idx：需评分代理索引（当前为空）。

city：城市名称（austin，美国奥斯汀市）。