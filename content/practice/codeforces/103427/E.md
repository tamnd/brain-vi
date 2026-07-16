---
title: "CF 103427E - Edward Gaming, Nhà vô địch"
description: "Chúng ta được cung cấp một chuỗi chữ thường duy nhất và được yêu cầu đếm số lần một mẫu cụ thể, cụ thể là chuỗi \"edgnb\", xuất hiện dưới dạng chuỗi con liền kề."
date: "2026-07-03T10:04:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103427
codeforces_index: "E"
codeforces_contest_name: "The 2021 ICPC Asia Shenyang Regional Contest"
rating: 0
weight: 103427
solve_time_s: 57
verified: true
draft: false
---

[CF 103427E - Nhà vô địch Edward Gaming](https://codeforces.com/problemset/problem/103427/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường và được yêu cầu đếm xem một mẫu cụ thể bao nhiêu lần, cụ thể là chuỗi`"edgnb"`, xuất hiện dưới dạng một chuỗi con liền kề. Mọi lần xuất hiện phải chính xác và không chồng chéo hoặc chồng chéo, cả hai đều được cho phép ngầm vì chúng tôi chỉ đếm tất cả các vị trí bắt đầu mà mẫu khớp hoàn toàn. 

Kích thước đầu vào có thể đạt tới 200.000 ký tự, điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào có hành vi bậc hai, chẳng hạn như kiểm tra rõ ràng mọi chuỗi con, đều không khả thi. Một trực tiếp$O(n \cdot 5)$quá trình quét vẫn ổn vì độ dài mẫu cố định và nhỏ, nhưng bất cứ điều gì liên tục xây dựng chuỗi con hoặc quét một cách đơn giản với chi phí bổ sung vẫn phải được kiểm soát cẩn thận để duy trì tuyến tính. 

Trường hợp phức tạp xuất phát từ các lần xuất hiện chồng chéo. Ví dụ: trong một chuỗi giả định như`"edgnbedgnb"`, có hai lần xuất hiện bắt đầu từ vị trí 0 và 5. Giải pháp đúng không được hợp nhất nhầm chúng hoặc bỏ qua phần chồng chéo. 

Một trường hợp cạnh khác là khi chuỗi ngắn hơn 5 ký tự. Trong trường hợp đó, câu trả lời nhất thiết là bằng không. Ví dụ, đầu vào`"edgn"`nên xuất ra`0`bởi vì nó không thể chứa mẫu đầy đủ. 

Cuối cùng, các chuỗi chứa các kết quả khớp một phần lặp đi lặp lại có thể đánh lừa các phương pháp quét ngây thơ khởi động lại không chính xác. Ví dụ,`"edgedgnb"`chỉ chứa một lần xuất hiện hợp lệ ở hậu tố; trận đấu một phần trước đó như`"edg"`hoặc`"edgn"`không được nhầm lẫn với các lượt truy cập hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi quét mọi vị trí$i$trong chuỗi và kiểm tra xem chuỗi con bắt đầu tại$i$trận đấu`"edgnb"`. Vì độ dài mẫu được cố định ở mức 5 nên mỗi lần kiểm tra là thời gian không đổi, tạo ra độ phức tạp tổng thể là$O(n)$thời gian. Thay vào đó, việc triển khai đơn giản có thể tạo các chuỗi con bằng cách cắt, cách này vẫn hoạt động trong Python đối với kích thước vấn đề này, nhưng việc phân bổ lặp lại có thể làm tăng các hệ số không đổi một cách không cần thiết. 

Cách tiếp cận vũ phu đã tối ưu về độ phức tạp tiệm cận vì mẫu có kích thước không đổi. Tuy nhiên, cải tiến về mặt khái niệm là nhận ra rằng chúng ta không bao giờ cần phải làm bất cứ điều gì phức tạp hơn việc so sánh cục bộ của năm ký tự. Không cần các hàm tiền tố, máy tự động hoặc băm cuộn vì mẫu cố định và ngắn. 

Do đó, vấn đề giảm xuống còn một lần quét tuyến tính với sự so sánh trực tiếp từng ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cắt chuỗi con) | O(n) | O(1) đến O(n) tùy theo việc cắt | Được chấp nhận nhưng nặng hơn | 
| Tối ưu (so sánh trực tiếp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào$s$. Chúng tôi sẽ quét nó từ trái sang phải và coi mỗi chỉ mục là điểm bắt đầu có thể có của mẫu. 
2. Xác định mẫu mục tiêu`"edgnb"`một lần. Điều này tránh việc xây dựng chuỗi lặp lại và làm cho việc so sánh trở nên rõ ràng. 
3. Lặp lại tất cả các chỉ số$i$từ 0 đến$n - 5$. Bất kỳ chỉ số nào ngoài$n - 5$không thể bắt đầu một trận đấu đầy đủ vì còn lại ít hơn 5 ký tự. 
4. Tại mỗi vị trí$i$, so sánh năm ký tự$s[i], s[i+1], s[i+2], s[i+3], s[i+4]$với các ký tự tương ứng của mẫu. Nếu tất cả đều khớp chính xác, hãy tăng câu trả lời. 
5. Xuất số đếm cuối cùng sau khi quá trình quét kết thúc. 

Ý tưởng chính là mỗi chỉ mục được xử lý độc lập. Chúng tôi không bao giờ cố gắng “mở rộng” kết quả khớp hoặc sử dụng lại tính toán từng phần vì mẫu đủ ngắn nên việc tính toán lại sẽ rẻ hơn so với việc duy trì trạng thái. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện của`"edgnb"`có một vị trí xuất phát duy nhất. Thuật toán kiểm tra mọi vị trí bắt đầu có thể có chính xác một lần và xác minh xem năm ký tự tiếp theo có khớp với mẫu hay không. Vì không thể bỏ sót sự kiện nào mà không bỏ qua chỉ mục bắt đầu của nó nên tính đầy đủ được đảm bảo. Vì chúng tôi chỉ tính khi tất cả các ký tự khớp chính xác nên độ chính xác được đảm bảo cho mỗi lần xuất hiện được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
pattern = "edgnb"

n = len(s)
ans = 0

for i in range(n - 4):
    if s[i] == 'e' and s[i+1] == 'd' and s[i+2] == 'g' and s[i+3] == 'n' and s[i+4] == 'b':
        ans += 1

print(ans)
```Việc triển khai sử dụng so sánh ký tự trực tiếp thay vì cắt lát. Điều này tránh việc tạo chuỗi tạm thời và giữ cho thời gian chạy tuyến tính nghiêm ngặt với chi phí không đổi ở mức tối thiểu. Vòng lặp bị ràng buộc`n - 4`đảm bảo chúng tôi không bao giờ truy cập các chỉ số ngoài phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"edgnb"`| tôi | đã kiểm tra chuỗi con | kết quả trận đấu | trả lời | 
| --- | --- | --- | --- | 
| 0 | edgnb | vâng | 1 | 

Vòng lặp chạy một lần và tìm thấy kết quả khớp hoàn toàn bắt đầu từ chỉ mục 0. Điều này xác nhận trường hợp đơn giản nhất trong đó chuỗi hoàn toàn bằng mẫu. 

### Ví dụ 2 

đầu vào:`"edgnbedgnb"`| tôi | đã kiểm tra chuỗi con | kết quả trận đấu | trả lời | 
| --- | --- | --- | --- | 
| 0 | edgnb | vâng | 1 | 
| 1 | dgnbe | không | 1 | 
| 2 | gnbed | không | 1 | 
| 3 | nbedg | không | 1 | 
| 4 | giường ngủ | không | 1 | 
| 5 | edgnb | vâng | 2 | 

Dấu vết này cho thấy sự chồng chéo được xử lý một cách tự nhiên vì mỗi chỉ mục được kiểm tra độc lập. Lần xuất hiện thứ hai bắt đầu ở chỉ số 5 và được tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua chuỗi với công việc không đổi trên mỗi vị trí | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 200.000 ký tự và quét tuyến tính với năm so sánh cho mỗi chỉ mục phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    pattern = "edgnb"

    n = len(s)
    ans = 0
    for i in range(n - 4):
        if s[i] == 'e' and s[i+1] == 'd' and s[i+2] == 'g' and s[i+3] == 'n' and s[i+4] == 'b':
            ans += 1

    return str(ans)

# provided sample
assert run("edgnb\n") == "1"

# custom cases
assert run("edgn\n") == "0", "too short"
assert run("aaaaaedgnb") == "1", "single match at end"
assert run("edgnbedgnb") == "2", "overlapping-independent matches"
assert run("edgnbedg") == "1", "partial suffix ignored"
assert run("edgnbedgnbedgnb") == "3", "multiple consecutive matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"edgn\n"`| 0 | chiều dài bên dưới mẫu | 
|`"aaaaaedgnb"`| 1 | khớp ở hậu tố | 
|`"edgnbedgnb"`| 2 | lần xuất hiện liên tiếp | 
|`"edgnbedg"`| 1 | phần kết quả khớp bị bỏ qua | 
|`"edgnbedgnbedgnb"`| 3 | xử lý mẫu lặp đi lặp lại | 

## Vỏ cạnh 

Đối với một chuỗi ngắn hơn 5 ký tự, chẳng hạn như`"abc"`hoặc`"edgn"`, vòng lặp`for i in range(n - 4)`không bao giờ thực thi vì`n - 4 <= 0`. Thuật toán ngay lập tức trả về 0, điều này đúng vì không thể tồn tại kết quả khớp đầy đủ. 

Đối với các mẫu chồng chéo như`"edgnbedgnb"`, quá trình quét sẽ kiểm tra mọi chỉ mục bắt đầu một cách độc lập. Ở chỉ số 0, nó khớp và ở chỉ số 5, nó khớp lại. Không có chỉ mục nào bị bỏ qua nên cả hai lần xuất hiện đều được tính. 

Đối với các chuỗi chứa tiền tố dài giống với mẫu nhưng sai lệch về sau, chẳng hạn như`"edgxxxxx"`, mỗi vị trí sẽ không sớm so sánh ký tự, ngăn chặn kết quả dương tính giả mà không cần phải quay lại hoặc theo dõi trạng thái.
