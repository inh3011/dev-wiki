# IAM: Users & Groups

## IAM?

- IAM은 Identity and Access Management의 약자이고, 사용자를 생성하고 그룹에 배치하기 때문에 글로벌 서비스이다.
- Root 계정을 생성하게 되면 기본으로 생성된다. 루트 계정은 계정을 생성할 때만 사용하고 그 후에는 유저를 생성해서 사용해야 한다.
- 사용자를 생성할 때, 사용자는 조직 내에 한 사람으로 해당 된다. 사용자들끼리 그룹을 묶을 수 있다. 

### Groups
- 6명의 사용자가 있다고 가정하자.
  - (A, B, C) 그룹
  - (C, D)그룹
  - (D, E) 그룹 
- 그룹에는 사용자만 포함 가능하고 그룹을 포함할 수 없다.
- 사용자는 그룹에 포함하지 않을 수 있는데, 권장하지 않는다.
- 사용자는 여러 그룹에 속할 수 있다.
  
### Why use Groups?
  - AWS 계정을 사용하도록 하기 위해서이다.
  - 허용을 위해서 권한을 부여해야 한다.


## IAM: Permissions?

- 사용자 또는 그룹에는 정책, IAM 정책이라고 불리는 JSON 문서를 할당할 수 있다.
- 이러한 정책은 사용자의 권한을 정의한다.  
- AWS에서는 최소 권한 원칙(Least Privilege Principle)을 적용해서, 사용자에게 필요한 권한 이상을 부여하지 않는다.
  - 사용자가 꼭 필요로 하는 것 이상의 권한을 주지 않는다.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

