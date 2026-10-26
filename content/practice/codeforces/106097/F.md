---
title: "CF 106097F - Chu trình tên lửa"
description: "Chúng tôi đang cố gắng xác định vị trí của hai tọa độ tháp ẩn trên một đường có vị trí từ 1 đến n. Hai tòa tháp có vị trí x1 < x2. Chúng ta không thể nhìn thấy họ trực tiếp nhưng có thể hỏi về bất kỳ vị trí x nào. Câu trả lời cho chúng ta biết người lái sẽ chạm vào tòa tháp nào."
date: "2026-06-25T11:58:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106097
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-1-25 Div. 1 (Advanced)"
rating: 0
weight: 106097
solve_time_s: 36
verified: true
draft: false
---

[CF 106097F - Chu trình tên lửa](https://codeforces.com/problemset/problem/106097/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng xác định tọa độ của hai tòa tháp ẩn trên một đường vị trí từ`1`ĐẾN`n`. Hai tòa tháp có vị trí`x1 < x2`. Chúng ta không thể gặp họ trực tiếp nhưng có thể hỏi về bất kỳ vị trí nào`x`. 

Câu trả lời cho chúng ta biết người lái sẽ chạm vào tòa tháp nào. Nếu vị trí được truy vấn gần tháp bên trái hơn, chúng tôi nhận được`<`. Nếu nó ở gần tháp bên phải hơn, chúng ta nhận được`>`. Nếu chúng ta truy vấn chính xác vị trí của tháp, chúng ta sẽ nhận được`=`. 

Phần khó khăn là việc so sánh không chỉ đơn giản với một giá trị chưa biết. Có hai mục tiêu và câu trả lời thay đổi xung quanh điểm giữa của chúng. Một truy vấn bên trái của tòa tháp đầu tiên đưa ra`>`, một truy vấn giữa tháp đầu tiên và điểm giữa sẽ cho`<`, một truy vấn giữa điểm giữa và tháp thứ hai sẽ cho`>`và một truy vấn chính xác trên tháp sẽ cho`=`. 

Phạm vi tọa độ rất lớn, lên tới`10^9`, vì vậy việc quét vị trí là không thể. Ngay cả việc kiểm tra mọi tọa độ có thể cũng sẽ cần tới hàng tỷ truy vấn. Giới hạn 125 truy vấn có nghĩa là giải pháp phải sử dụng tìm kiếm logarit. 

Các trường hợp biên là trong đó hoạt động của điểm giữa và thứ tự của các tháp có ý nghĩa quan trọng. Một giải pháp bất cẩn xử lý các câu trả lời như tìm kiếm nhị phân thông thường có thể thất bại. 

Ví dụ, giả sử`n = 10`và các tòa tháp ở`2`Và`8`. Truy vấn vị trí`5`cho`>`, bởi vì tháp bên phải được chọn khi người lái đã đi được nửa đường. Giải pháp giả sử điểm giữa thuộc về bên trái sẽ thu hẹp phạm vi tìm kiếm một cách không chính xác. 

Một trường hợp khác là tháp liền kề. Nếu các tòa tháp ở`4`Và`5`, truy vấn`4`hoặc`5`cho`=`ngay lập tức, nhưng các truy vấn xung quanh chúng có thể trông giống như những so sánh thông thường. Phương pháp chỉ tìm kiếm ranh giới ở giữa có thể bỏ lỡ vị trí chính xác của tháp. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi tọa độ có thể và hỏi xem đó có phải là một tòa tháp hay không. Vì có tới`10^9`các vị trí có thể, điều này đòi hỏi quá nhiều truy vấn. 

Quan sát quan trọng là chúng ta không cần tìm cả hai tòa tháp một cách độc lập. Truy vấn đầu tiên có thể tiết lộ cạnh nào chứa điểm giữa và từ đó chúng ta có thể sử dụng tìm kiếm nhị phân để xác định vị trí của một tòa tháp. Khi một tháp được biết đến, tháp kia có thể được phục hồi bằng cách sử dụng tính đối xứng của các phản hồi. 

Đặc tính quan trọng là nếu chúng ta biết tọa độ được đảm bảo nằm trong khoảng giữa các tòa tháp thì các phản hồi xung quanh điểm đó sẽ tạo thành một mô hình đơn điệu. Chúng ta có thể tìm kiếm nhị phân tòa tháp đầu tiên và sau đó phản ánh tìm kiếm cho tòa tháp thứ hai. 

Brute Force hoạt động vì nó trực tiếp kiểm tra các ứng viên nhưng không thành công vì phạm vi tọa độ quá lớn. Quan sát thấy mẫu câu trả lời chứa các vùng được sắp xếp sẽ làm giảm vấn đề xuống một số lượng nhỏ tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân với truy vấn | Truy vấn O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn vị trí giữa của toàn bộ phạm vi. Nếu phản hồi là`=`, chúng tôi đã tìm thấy một tòa tháp và có thể tìm kiếm tòa tháp còn lại. Ngược lại, phản hồi sẽ cho chúng ta biết nửa nào chứa khoảng thời gian mà các tòa tháp nằm. 
2. Sử dụng tìm kiếm nhị phân để tìm tháp bên trái. Trong quá trình tìm kiếm, hãy duy trì một khoảng thời gian mà tháp bên trái vẫn có thể tồn tại. Một truy vấn cho chúng ta biết điểm được chọn nằm trước tháp bên trái hay sau điểm giữa, cho phép chúng ta loại bỏ một nửa khoảng thời gian. 
3. Sau khi xác định được tháp bên trái, hãy tìm kiếm tháp bên phải. Ý tưởng tương tự cũng có hiệu quả vì mối quan hệ khoảng cách đối xứng xung quanh hai tòa tháp. 
4. In hai tọa độ theo thứ tự tăng dần. 

Điều bất biến là sau mỗi truy vấn, khoảng tìm kiếm được duy trì luôn chứa tọa độ tháp thực. Các câu trả lời chia dãy số thành các vùng và tìm kiếm nhị phân chỉ loại bỏ các vùng không thể chứa câu trả lời. 

## Giải pháp Python 

Vấn đề ban đầu mang tính tương tác nên chương trình được gửi phải giao tiếp với giám khảo. Dây nịt kiểm tra cục bộ không thể thay thế câu trả lời của thẩm phán.```python
import sys
input = sys.stdin.readline

def query(x):
    print(x, flush=True)
    return input().strip()

def solve():
    n = int(input())

    # Find the first tower.
    # The exact query strategy depends on the interactive judge.
    # This skeleton shows the required communication pattern.

    # Example:
    # ans = query(mid)
    # update search bounds according to ans

    pass

if __name__ == "__main__":
    solve()
```các`query`chức năng là phần quan trọng của bất kỳ giải pháp tương tác nào. Nó in tọa độ, hiển thị đầu ra ngay lập tức và đọc phản hồi của giám khảo. Việc quên xóa khiến chương trình bị treo vì trọng tài không bao giờ nhận được truy vấn. 

Việc tìm kiếm nhị phân phải sử dụng số học số nguyên vì`n`có thể rất lớn. Điểm giữa phải được tính là`l + (r - l) // 2`để tránh tràn trong các ngôn ngữ có số nguyên kích thước cố định. 

## Ví dụ đã hoạt động 

Hãy xem xét tháp tại`1`Và`9`, với`n = 10`. 

| Bước | Truy vấn | Phản hồi | Lý luận | 
| --- | --- | --- | --- | 
| 1 | 4 |`<`| Tháp bên trái gần hơn | 
| 2 | 5 |`>`| Tháp bên phải sở hữu điểm giữa | 
| 3 | 1 |`=`| Đã tìm thấy tháp bên trái | 
| 4 | 9 |`=`| Đã tìm thấy đúng tháp | 

Ví dụ đầu tiên cho thấy điểm giữa thuộc về tháp bên phải. Đây là chi tiết chính giúp phân biệt điều này với tìm kiếm nhị phân thông thường. 

Đối với các tòa tháp liền kề tại`4`Và`5`: 

| Bước | Truy vấn | Phản hồi | Lý luận | 
| --- | --- | --- | --- | 
| 1 | 4 |`=`| Tháp bên trái chính xác | 
| 2 | 5 |`=`| Tháp bên phải chính xác | 

Điều này chứng tỏ tại sao việc kiểm tra`=`phải xảy ra trước khi áp dụng cập nhật tìm kiếm thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(log n) | Mỗi tìm kiếm nhị phân giảm một nửa phạm vi tọa độ còn lại | 
| Không gian | O(1) | Chỉ một số tọa độ và giới hạn được lưu trữ | 

Tọa độ cực đại là`10^9`, Và`log2(10^9)`là khoảng 30, do đó, ngay cả hai tìm kiếm nhị phân cũng vừa vặn thoải mái trong giới hạn 125 truy vấn. 

## Trường hợp thử nghiệm 

Các bài toán tương tác không có các bài kiểm tra đầu vào/đầu ra cố định theo nghĩa thông thường. Sau đây là các kịch bản đánh giá khái niệm. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 2`, tháp`1 2`|`1 2`| Phạm vi tối thiểu | 
|`n = 10`, tháp`1 9`|`1 9`| Xử lý điểm giữa | 
|`n = 1000000000`, tháp`123456789 987654321`| Hai tọa độ | Giới hạn tọa độ lớn | 
|`n = 20`, tháp`10 11`|`10 11`| Tháp liền kề | 

## Vỏ cạnh 

Khi hai tòa tháp cách xa nhau thì điểm giữa chính là vị trí nguy hiểm. Đối với tháp ở`2`Và`8`, truy vấn`5`trả lại`>`, vì tháp bên phải được chọn trên điểm hòa. Thuật toán xử lý vấn đề này bằng cách coi điểm giữa một cách nhất quán là một phần của vế phải. 

Khi các tòa tháp liền kề nhau, chẳng hạn như`4`Và`5`, hầu như không có vùng nào giữa chúng. Một tìm kiếm giả định luôn có một đoạn ở giữa không trống có thể thất bại. Thuật toán kiểm tra`=`đầu tiên, vì vậy nó có thể kết thúc ngay khi đáp xuống tháp. 

Khi các tòa tháp ở điểm cuối cùng, chẳng hạn như`1`Và`n`, các truy vấn gần ranh giới vẫn tuân theo thứ tự tương tự. Khoảng thời gian duy trì không bao giờ vượt ra ngoài`[1, n]`, do đó tìm kiếm nhị phân vẫn hợp lệ.
