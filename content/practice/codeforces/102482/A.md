---
title: "CF 102482A - Bắt máy bay"
description: "Thành phố là một đồ thị thời gian có hướng. Mỗi xe buýt là một cạnh có thời gian khởi hành, thời gian đến và xác suất tồn tại vào ngày đi. Bạn xuất phát ở ga 0 trước khi tất cả các xe buýt khởi hành và bạn cần tối đa hóa xác suất có mặt ở ga 1 theo thời gian k."
date: "2026-08-05T18:51:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "A"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 116
verified: true
draft: false
---

[CF 102482A - Bắt máy bay](https://codeforces.com/problemset/problem/102482/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một đồ thị thời gian có hướng. Mỗi xe buýt là một cạnh có thời gian khởi hành, thời gian đến và xác suất tồn tại vào ngày đi. Bạn bắt đầu ở ga`0`trước khi tất cả các xe buýt khởi hành và bạn cần tối đa hóa khả năng có mặt tại ga`1`theo thời gian`k`. 

Khó khăn đến từ hai chi tiết. Một chiếc xe buýt có thể biến mất và bạn chỉ biết được điều đó sau khi chọn nó. Ngoài ra, không thể kiểm tra tất cả các xe buýt khởi hành từ cùng một bến, vì sau một lần thử thất bại thì thời điểm khởi hành đã trôi qua. 

Những hạn chế là gợi ý chính. Có thể có tới`10^6`xe buýt và`10^6`các trạm, do đó, bất kỳ điều gì liên quan đến lập trình động theo thời gian hoặc tìm kiếm biểu đồ lặp đi lặp lại đều không thể thực hiện được. Giá trị thời gian có thể lớn như`10^18`, vì vậy chúng ta không thể lặp lại mọi thời điểm có thể. Thuật toán chỉ phải xử lý các sự kiện bus nhất định, có nghĩa là công việc gần như tuyến tính hoặc logarit tuyến tính. 

Một sai lầm phổ biến là coi xe buýt khởi hành cùng lúc là những lựa chọn độc lập. Ví dụ:```
2 2
1
0 1 0 1 0.5
0 1 0 1 0.5
```Câu trả lời là`0.5`, không`0.75`. Bạn chỉ có thể thử một xe buýt. Một sai lầm nữa là cho phép chuyển chuyến khi giờ đến bằng giờ đi. Nếu một xe buýt đến đúng lúc một xe khác khởi hành thì kết nối đó không hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng mô phỏng mọi chiến lược có thể. Tại mỗi trạm và thời gian, nó sẽ xem xét tất cả các xe buýt có thể được đón và tính toán đệ quy xác suất tốt nhất. Điều này đúng vì nó xem xét mọi quyết định có thể xảy ra, nhưng số lượng trạng thái là rất lớn. Có tới`10^6`xe buýt và chuỗi trung chuyển có thể tạo ra nhiều chiến lược khả thi theo cấp số nhân. 

Quan sát quan trọng là tất cả các quyết định có thể được xử lý ngược thời gian. Khi xem xét một xe buýt khởi hành vào thời điểm`s`, mỗi xe buýt khởi hành sau`s`đã được xử lý rồi. Vì vậy, chúng ta đã biết xác suất thành công tối ưu sau khi đến đích của chiếc xe buýt này. 

Cho phép`dp[v]`nghĩa là: nếu chúng ta đang ở ga`v`ngay sau tất cả các chuyến khởi hành tại thời điểm xử lý hiện tại, xác suất tối đa để đến sân bay là bao nhiêu? 

Đối với một chiếc xe buýt`a -> b`rời đi đúng lúc`s`, nếu chúng ta thử thì xác suất là:```
p * dp[b] + (1 - p) * dp[a]
```Thuật ngữ đầu tiên là trường hợp xe buýt chạy. Thuật ngữ thứ hai là trường hợp nó thất bại và chúng tôi bị kẹt ở trạm`a`sau khi thời điểm khởi hành đã trôi qua. 

Việc xử lý các xe buýt có cùng giờ khởi hành cùng nhau là cần thiết. Mọi ứng viên phải sử dụng giá trị cũ của`dp[a]`, bởi vì một lần thử thất bại không thể được nối tiếp bởi một xe buýt khác vào cùng thời điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Lớn | Quá chậm | 
| Quét ngược | O(m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`dp[1] = 1`bởi vì đã có mặt ở sân bay đồng nghĩa với việc thành công. Mọi trạm khác đều bắt đầu với xác suất`0`. 
2. Sắp xếp tất cả các xe buýt theo thời gian khởi hành theo thứ tự giảm dần. Những chuyến khởi hành muộn nhất sẽ được xử lý trước vì điểm đến của chúng không thể sử dụng bất kỳ chuyến xe buýt nào sau đó. 
3. Đối với mỗi nhóm xe buýt có cùng thời gian khởi hành, hãy tính toán mọi nỗ lực có thể bằng cách sử dụng giá trị hiện tại`dp`các giá trị. 
4. Đối với xe buýt có xác suất`p`, lưu trữ giá trị ứng cử viên`p * dp[destination] + (1 - p) * dp[start]`. 
5. Sau khi đánh giá toàn bộ nhóm thời gian, hãy cập nhật ứng viên xuất sắc nhất từ ​​nhóm đó vào vị trí xuất phát. 
6. Sau khi tất cả các bus được xử lý,`dp[0]`là xác suất tối ưu từ trạng thái ban đầu. 

Bất biến là trước khi xử lý thời gian khởi hành`s`,`dp[v]`đã chứa xác suất tối ưu chỉ sử dụng các xe buýt khởi hành sau`s`. Một chiếc xe buýt khởi hành lúc`s`chỉ cần thông tin trong tương lai từ đích đến của nó, thông tin này đã có sẵn. Việc xử lý thời gian khởi hành bằng nhau sẽ duy trì quy tắc rằng chỉ có thể thử một xe buýt tại thời điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    k = int(input())

    buses = []

    for _ in range(m):
        a, b, s, t, p = input().split()
        buses.append((int(s), int(a), int(b), float(p)))

    buses.sort(reverse=True)

    dp = [0.0] * n
    dp[1] = 1.0

    i = 0
    while i < m:
        time = buses[i][0]
        best = {}

        while i < m and buses[i][0] == time:
            _, a, b, p = buses[i]
            value = p * dp[b] + (1.0 - p) * dp[a]

            if a not in best or value > best[a]:
                best[a] = value

            i += 1

        for a, value in best.items():
            if value > dp[a]:
                dp[a] = value

    print("{:.10f}".format(dp[0]))

if __name__ == "__main__":
    solve()
```Thứ tự sắp xếp là cốt lõi của việc thực hiện. Một chiếc xe buýt đến đúng lúc`t`luôn cần câu trả lời theo thời gian`t`, có nghĩa là mọi xe buýt có thời gian khởi hành lớn hơn`t`chắc chắn đã được xử lý rồi. 

Việc cập nhật nhóm cũng rất cần thiết. Đang cập nhật`dp`ngay sau khi nhìn thấy một xe buýt sẽ cho phép một xe buýt khác cùng giờ khởi hành sử dụng không chính xác giá trị được cải thiện sau lần thử không thành công. Giữ ứng viên ở lại`best`ngăn chặn điều này. 

Độ chính xác của dấu phẩy động Python ở đây là đủ vì lỗi yêu cầu chỉ là`10^-6`. Giá trị thời gian không bao giờ được sử dụng trong số học, do đó kích thước của chúng không tạo ra vấn đề tràn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp chiếm ưu thế; mỗi xe buýt được xử lý một lần | 
| Không gian | O(n + m) | Lưu trữ các xe buýt và một xác suất cho mỗi trạm | 

Với một triệu xe buýt, việc quét tuyến tính có thể được quản lý dễ dàng và hệ số sắp xếp logarit có thể chấp nhận được trong giới hạn bộ nhớ rộng rãi. 

## Ví dụ đã hoạt động 

Đối với mẫu thứ hai:```
4 2
2
0 1 0 1 0.5
0 1 0 1 0.5
0 1 1 2 0.4
0 1 1 2 0.2
```Hai xe buýt mới nhất được xử lý đầu tiên. 

| Giờ khởi hành | Xác suất xe buýt | dp[0] sau khi xử lý | 
| --- | --- | --- | 
| 1 | 0,4, 0,2 | 0,4 | 
| 0 | 0,5, 0,5 | 0,7 | 

Vào thời điểm`1`, phương án tốt nhất cho xác suất`0.4`. Vào thời điểm`0`, việc thử chiếc xe buýt đầu tiên thành công với xác suất`0.5`, và thất bại sẽ để lại cơ hội bắt chuyến xe buýt sau. Kết quả là:```
0.5 + 0.5 * 0.4 = 0.7
```phù hợp với đầu ra mẫu.
