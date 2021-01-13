---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터의 이해와 데이터베이스
draft: false
title: NoSQL (DynamoDB)
weight: 9
---

## Part 4. 데이터의 이해와 데이터베이스
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

이 포스팅은 NoSQL 중 DynamoDB를 위주로 서술되어 있습니다.

### 1. NoSQL vs. RDB
- `Not Only SQL`
- 차이점(1) 다이나믹 스키마
    - 구조를 정의하지 않고도 Documents, Key Values 등을 생성
    - 각각의 Document가 서로 다른 구조로 구성 가능
    - 데이터베이스들마다 다른 syntax
    - 필드 추가 가능
- 차이점(2) Scalabilty
    - SQL DB: `vertically` scalable - CPU, RAM, SSD로 용량 문제 해결결
    - NoSQL DB: `horizontally` scalable - Sharding, Partitioning로 용량 문제 해결

### 2. Partition
- 데이터 나누기(vertical & horizontal)
    - 데이터 매니지먼트, 퍼포먼스 등 다양한 이유
1. Vertical Partition
    - 테이블을 더 작은 테이블로 나누기(Normalization와는 다름)
    - ex. 지속적으로 업데이트되는 칼럼과 아닌 칼럼들 나누기
2. Horizontal Partition
    - Schema / Structure 자체를 복사하여 데이터 자체를 `Sharded Key`로 분리
    - NosQL DB에서는 필수적이다.
    
### 3. DynamoDB
- aws.amazon.com > DynamoDB
- Partition Key는 SQL에서 Primary Key와 유사하다.

### 4. AWS SDK - Boto3 Package (DynamoDB 연결)
```python
import sys
import os
import boto3
import logging

def main():
    try:
        dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2', endpoint_url='http://dynamodb.ap-northeast-2.amazonaws.com')
    except:
        logging.error("could not connect to dynamodb")
        sys.exit(1)

    print('Success')

if __name__=='__main__':
    main()
```

### 5. 테이블 생성 및 스펙
- Provisioned(할당됨) vs. On-demand(온디맨드)

### 6. Global Index, Local Index

### 7. INSERT(Single, Batch items)
[`boto3 Documentation`](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/dynamodb.html) 읽어보기

###### Creating a New Item
```python
table.put_item(
   Item={
        'username': 'janedoe',
        'first_name': 'Jane',
        'last_name': 'Doe',
        'age': 25,
        'account_type': 'standard_user',
    }
)
```

###### Batch Writing
```python
with table.batch_writer() as batch:
    batch.put_item(
        Item={
            'account_type': 'standard_user',
            'username': 'johndoe',
            'first_name': 'John',
            'last_name': 'Doe',
            'age': 25,
            'address': {
                'road': '1 Jefferson Street',
                'city': 'Los Angeles',
                'state': 'CA',
                'zipcode': 90001
            }
        }
    )
    batch.put_item(
        Item={
            'account_type': 'super_user',
            'username': 'janedoering',
            'first_name': 'Jane',
            'last_name': 'Doering',
            'age': 40,
            'address': {
                'road': '2 Washington Avenue',
                'city': 'Seattle',
                'state': 'WA',
                'zipcode': 98109
            }
        }
    )
    batch.put_item(
        Item={
            'account_type': 'standard_user',
            'username': 'bobsmith',
            'first_name': 'Bob',
            'last_name':  'Smith',
            'age': 18,
            'address': {
                'road': '3 Madison Lane',
                'city': 'Louisville',
                'state': 'KY',
                'zipcode': 40213
            }
        }
    )
    batch.put_item(
        Item={
            'account_type': 'super_user',
            'username': 'alicedoe',
            'first_name': 'Alice',
            'last_name': 'Doe',
            'age': 27,
            'address': {
                'road': '1 Jefferson Street',
                'city': 'Los Angeles',
                'state': 'CA',
                'zipcode': 90001
            }
        }
    )
```

### 8. 데이터 요청 및 제한점
[`boto3 Documentation`](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/dynamodb.html) 읽어보기

###### Getting an Item
```python
response = table.get_item(
    Key = {
        'artist_id': '00FQb4jTyendYWaN8pK0wa',
        'id': '0Oqc0kKFsQ6MhFOLBNZIGX'
    }
)
```
```{error}
ClientError: An error occurred (ValidationException) when calling the GetItem operation: The provided key element does not match the schema
```
위와 같은 에러가 뜬다면, key값을 제대로 다 넣었는지 확인해본다.

###### Querying and Scanning
- Querying: Primary Key 값을 알고 있을 때 활용
- Scanning: Primary Key 값을 모르지만, 다른 attribute를 알 때 활용
        - Scan은 모든 행을 다 훑는 비효율적인 기능이므로 꼭 필요할 때만 쓰는 것이 권장된다.

```python
# Querying
response = table.query(
    KeyConditionExpression=Key('artist_id').eq('00FQb4jTyendYWaN8pK0wa'),
    FilterExpression=Attr('popularity').gt(75) #query도 filterexpresson 쓸 수 있다!
)
print(len(response['Items']))

# Scanning
response = table.scan(
    FilterExpression=Attr('popularity').gt(75)
)
print(len(response['Items']))
```
<br>