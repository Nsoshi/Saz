import boto3
import json

# DynamoDBクライアントを作成
dynamodb = boto3.client('dynamodb')

# SNSクライアントを作成
sns = boto3.client('sns')

def lambda_handler(event, context):
    # テーブル名とキーの値を指定
    table_name = 'test_read'
    timestamp_value = 'a'
    id_value = 'b'
    threshold = 10  # 閾値を設定

    # データを取得
    response = dynamodb.get_item(
        TableName=table_name,
        Key={
            'timestamp': {'S': timestamp_value},
            'ID': {'S': id_value}
        }
    )

    # 必要なデータを処理
    item = response.get('Item')
    if item:
        # データが存在する場合の処理
        print(item)
        
        value = item.get('value').get('N')  # 数値データの取得

        # メールの内容を条件に応じて変更
        if int(value) > threshold:
            message = f'値が閾値を超えています: {value}'
        else:
            message = f'値は閾値以下です: {value}'

        # メールの送信
        subject = 'データが見つかりました'
        response = sns.publish(
            TopicArn='arn:aws:sns:ap-northeast-1:708546219575:Twitter_log',
            Message=message,
            Subject=subject
        )

        print('メールが送信されました')

        pass
    else:
        # データが存在しない場合の処理
        pass

    # レスポンスを返す
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Data retrieved successfully'
        })
    }
