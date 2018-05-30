## 获取免费课程接口

* 接口地址：`free_course`/

* GET 请求：0元课程免费领取

  * 请求参数：

    * `userId` 用户ID
    * `courseId` 课程ID

  * 返回数据：

    * 400：

      ```json
      {'retCode': 10000,'retMsg': '请求参数错误'}
      {'retCode': 30101,'retMsg': '用户不存在'}
      {'retCode': 40028,'retMsg': '课程不存在'}
      {'retCode': 40034,'retMsg': '课程价格不正确'}
      {'retCode': 40031,'retMsg': '课程购买人数超限'}
      ```

    * 500:

      ```json
      {'retCode': 20000,'retMsg': '保存数据错误'}
      ```

    * 200:

      ```json
      {'retCode': 0,'retMsg': '成功 | Success'}
      ```

      ​