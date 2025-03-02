## Stolon의 주요 구성 요소

### Sentinel
- PostgreSQL 인스턴스 상태를 지속적으로 모니터링함
- 마스터(Primary) 장애 발생 시 새로운 마스터를 선출하여 Failover 수행

### Keeper
- PostgreSQL 인스턴스를 직접 관리하는 구성 요소
- 새로운 마스터가 선출되면 해당 설정을 반영하여 PostgreSQL을 재구성함

### Proxy
- 클라이언트의 연결을 현재 마스터로 자동으로 라우팅
- 마스터 변경 시 클라이언트가 수동으로 접속 정보를 변경할 필요 없음

### Consul/etcd (저장소)
- 클러스터 상태 및 메타데이터를 저장하는 분산 키-값 저장소
- Sentinel과 Keeper가 상태를 공유하고 조정하는 데 사용됨