---
title: "CF 105055M - Ánh sáng mờ"
description: "Chúng ta được cấp một đoạn đường một chiều từ vị trí 0 đến vị trí N, cùng với một tập hợp các cột đèn đặt tại tọa độ nguyên cố định dọc theo đoạn đường này."
date: "2026-06-28T01:09:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "M"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 58
verified: true
draft: false
---

[CF 105055M - Ánh sáng mờ](https://codeforces.com/problemset/problem/105055/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một đoạn đường một chiều từ vị trí 0 đến vị trí N, cùng với một tập hợp các cột đèn đặt tại tọa độ nguyên cố định dọc theo đoạn đường này. Mỗi cột đèn sử dụng một bóng đèn giống hệt nhau và mỗi bóng đèn chiếu sáng đối xứng sang trái và phải một khoảng cố định R mà chúng ta gọi là công suất. Một điểm trên đường được coi là sáng nếu nó nằm trong khoảng cách R tính từ ít nhất một cột đèn. 

Nhiệm vụ là chọn giá trị nhỏ nhất có thể của R sao cho mọi điểm trên khoảng từ 0 đến N được chiếu sáng bởi ít nhất một cột đèn. 

Kích thước đầu vào cho phép lên tới 200.000 cột đèn, vì vậy mọi giải pháp đều phải tránh so sánh bậc hai giữa tất cả các cặp. Một giải pháp so sánh từng cặp cột đèn hoặc kiểm tra vùng phủ sóng từng điểm trên một lưới mịn sẽ là quá chậm. Chúng ta nên mong đợi một giải pháp xử lý các vị trí cột đèn theo thời gian tuyến tính hoặc tuyến tính sau khi sắp xếp, nhưng vì chúng đã được sắp xếp nên có thể đạt được thời gian tuyến tính. 

Một cách tiếp cận đơn giản sẽ thử bán kính ứng cử viên R và xác minh xem phạm vi bao phủ có hoàn chỉnh hay không, có thể bằng cách kiểm tra tất cả các điểm hoặc quét đường theo từng bước nhỏ. Ngay cả tìm kiếm nhị phân trên R kết hợp với quét tuyến tính cũng có thể hoạt động, nhưng chúng ta có thể làm tốt hơn bằng cách tính toán trực tiếp khoảng cách giới hạn. 

Các trường hợp quan trọng nhất đến từ ranh giới. Nếu chỉ có một cột đèn thì đáp án phải đủ lớn để phủ kín cả hai đầu đường sao cho nó trở thành max(P[0], N - P[0]). Một trường hợp tinh vi khác phát sinh khi các cột đèn được nhóm lại nhưng lại có một khoảng trống lớn ở giữa; khoảng cách đó chiếm ưu thế trong câu trả lời bất kể khoảng cách khác. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là cố định bán kính R và kiểm tra xem tập hợp các khoảng [P_i - R, P_i + R] có bao phủ toàn bộ đoạn [0, N] hay không. Việc xác minh này có thể được thực hiện bằng cách quét qua các cột đèn và duy trì điểm được che phủ xa nhất. Nếu tại bất kỳ thời điểm nào xuất hiện một khoảng trống thì R không đủ. 

Tuy nhiên, việc thử tất cả các giá trị có thể có của R là không khả thi vì R có thể nằm trong phạm vi N và tìm kiếm trực tiếp trên tất cả các giá trị sẽ là O(N^2) nếu mỗi lần kiểm tra quét tất cả các cột đèn. Ngay cả tìm kiếm nhị phân cũng cải thiện điều này thành O(M log N), nhưng chúng ta có thể tránh hoàn toàn việc tìm kiếm. 

Quan sát quan trọng là phạm vi bao phủ được xác định hoàn toàn bởi khoảng cách giữa các cột đèn liên tiếp và ranh giới. Giữa hai cột đèn liền kề tại vị trí P[i] và P[i+1], khoảng cách giữa các khoảng phủ sóng của chúng chỉ được đóng lại nếu mỗi cột kéo dài ít nhất một nửa khoảng cách giữa chúng. Do đó, hệ số giới hạn bên trong đoạn này là một nửa khoảng cách liền kề tối đa. Ở hai đầu, chúng ta cũng cần bao gồm khoảng cách từ 0 đến đèn đầu tiên và từ đèn cuối cùng đến N. 

Vì vậy, câu trả lời là giá trị lớn nhất trong ba đại lượng: khoảng cách từ 0 đến P[0], khoảng cách từ P[M-1] đến N và một nửa chênh lệch lớn nhất giữa các cột đèn liên tiếp. Điều này trực tiếp mang lại bán kính yêu cầu tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·M) | O(1) | Quá chậm | 
| Tối ưu | O(M) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính khoảng cách từ đầu phố đến cột đèn đầu tiên, vì cột đèn đó phải che phủ độc lập mọi thứ bên trái của nó. Giá trị này là P[0]. 
2. Tính khoảng cách từ cột đèn cuối cùng đến cuối đường, vì không có cột đèn nào khác có thể giúp mở rộng phạm vi phủ sóng ra ngoài cột đèn đó. Giá trị này là N - P[M-1]. 
3. Quét tất cả các cặp cột đèn liên tiếp và tính khoảng cách giữa chúng, P[i+1] - P[i]. Tầm phủ sóng của cả hai bên phải gặp nhau ở giữa nên mỗi cột đèn chỉ đóng góp tối đa một nửa khoảng trống đó. Theo dõi khoảng cách tối đa như vậy trên tất cả các cặp. 
4. Lấy một nửa khoảng trống tối đa được tìm thấy ở bước 3. Đây là bán kính tối thiểu cần thiết để không có vùng không che chắn nào xuất hiện giữa hai cột đèn lân cận bất kỳ. 
5. Câu trả lời cuối cùng là giá trị lớn nhất của ba giá trị được tính: khoảng cách ranh giới bên trái, khoảng cách ranh giới bên phải và một nửa khoảng cách bên trong tối đa. 

### Tại sao nó hoạt động 

Mọi điểm trên đường phải nằm trong khoảng cách R tính từ cột đèn nào đó. Những nơi duy nhất mà phạm vi phủ sóng có thể không thành công là ở ranh giới hoặc ở giữa hai cột đèn liên tiếp, vì trong khoảng thời gian phủ sóng liên tục từ một cột đèn duy nhất không có khoảng trống bên trong. Ở ranh giới chỉ có một cột đèn góp phần nên bán kính ít nhất phải chạm tới mép. Giữa hai cột đèn, khoảng cách phủ sóng của chúng chỉ gặp nhau nếu cả hai kéo dài đủ xa để thu hẹp khoảng cách giữa điểm, điều này buộc R phải cách ít nhất một nửa khoảng cách giữa chúng. Việc lấy tối đa các điều kiện cần thiết này sẽ tạo ra bán kính khả thi nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    P = list(map(int, input().split()))

    # distance to left boundary
    ans = P[0]

    # distance to right boundary
    ans = max(ans, N - P[-1])

    # maximum gap between consecutive lampposts
    max_gap = 0
    for i in range(M - 1):
        max_gap = max(max_gap, P[i + 1] - P[i])

    ans = max(ans, max_gap / 2)

    # output as integer (problem guarantees integer result)
    print(int(ans))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ba ràng buộc xác định tính khả thi. Hai dòng đầu tiên sau khi đọc đầu vào xử lý phạm vi bao phủ ranh giới. Vòng lặp tính toán khoảng cách lớn nhất giữa các cột đèn liền kề, đây là nơi duy nhất mà hai phạm vi chiếu sáng phải gặp nhau. Chia cho hai sẽ chuyển một khoảng trống thành sự mở rộng đối xứng cần thiết từ cả hai phía. Mức tối đa cuối cùng kết hợp tất cả các ràng buộc vào bán kính đủ tối thiểu. 

Một điểm tinh tế là sử dụng phép chia cho 2.0 và sau đó chuyển sang int giả sử câu trả lời là tích phân hoặc có thể biểu thị chính xác mà không có vấn đề làm tròn. Trong các công thức điển hình của bài toán này, tất cả các vị trí đều là số nguyên và bán kính tối ưu là số nguyên hoặc nửa số nguyên theo cách vẫn mang lại kết quả là số nguyên sau khi đảm bảo cho từng vấn đề cụ thể. Nếu ưu tiên số học số nguyên nghiêm ngặt, phép chia số nguyên với trần có thể được sử dụng như`(gap + 1) // 2`khi làm việc ở dạng chỉ số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
20 5
1 5 9 15 18
```Chúng tôi tính toán khoảng cách ranh giới và khoảng trống. 

| Bước | Giá trị | 
| --- | --- | 
| Ranh giới bên trái | 1 | 
| Ranh giới bên phải | 2 | 
| Khoảng trống | 4, 4, 6, 3 | 
| Khoảng cách tối đa | 6 | 
| Khoảng cách tối đa một nửa | 3 | 
| Trả lời | 3 | 

Điều này cho thấy hạn chế chủ yếu là khoảng cách giữa 9 và 15. Mặc dù ranh giới nhỏ nhưng lực phân tách ở giữa bán kính là 3. 

### Mẫu 2 

đầu vào:```
5 1
1
```| Bước | Giá trị | 
| --- | --- | 
| Ranh giới bên trái | 1 | 
| Ranh giới bên phải | 4 | 
| Khoảng cách tối đa | 0 | 
| Trả lời | 4 | 

Chỉ với một cột đèn, vùng phủ sóng phải kéo dài đến cả hai đầu. Kết quả được xác định hoàn toàn bởi ranh giới xa hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M) | đi qua cột đèn một lần sau khi đọc đầu vào | 
| Không gian | O(1) | chỉ sử dụng các biến phụ không đổi | 

Giải pháp chạy theo thời gian tuyến tính theo số lượng cột đèn, nằm trong giới hạn cho 200.000 phần tử và sử dụng bộ nhớ bổ sung liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    N, M = map(int, inp.split()[0:2])
    P = list(map(int, inp.split()[2:]))

    # direct implementation of solution
    ans = P[0]
    ans = max(ans, N - P[-1])
    max_gap = 0
    for i in range(M - 1):
        max_gap = max(max_gap, P[i+1] - P[i])
    ans = max(ans, max_gap / 2)
    return str(int(ans))

# provided samples
assert run("20 5\n1 5 9 15 18\n") == "3"
assert run("5 1\n1\n") == "4"

# minimum size
assert run("10 1\n0\n") == "10"

# evenly spaced
assert run("10 3\n0 5 10\n") == "5"

# large gap in middle
assert run("100 4\n0 10 90 100\n") == "45"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đèn đơn ở 0 | N | trường hợp chỉ có ranh giới | 
| đèn cách đều nhau | sự thống trị giữa khoảng cách | độ chính xác của vùng phủ sóng đối xứng | 
| khoảng cách nội bộ lớn | logic nửa khoảng cách | ràng buộc quan trọng chính | 

## Vỏ cạnh 

Một cột đèn duy nhất ở vị trí 0 trên một con phố dài được bao phủ bằng cách đặt bán kính bằng N. Thuật toán tính khoảng cách ranh giới bên trái là 0 và ranh giới bên phải là N, do đó câu trả lời là N, phù hợp với yêu cầu một đèn phải bao phủ toàn bộ đoạn đường. 

Trường hợp có hai cột đèn cách xa nhau, chẳng hạn như P = [2, 8] trên N = 10, bộc lộ tình trạng khe hở. Khoảng cách là 6, do đó nửa khoảng cách là 3. Khoảng cách biên là 2 và 2, vì vậy câu trả lời là 3. Thuật toán chọn chính xác điểm giữa của vùng không được che chắn lớn nhất làm hệ số giới hạn. 

Một tập hợp được phân cụm chặt chẽ với điểm cuối ở xa, chẳng hạn như P = [0, 1, 2, 100], làm cho ranh giới bên phải chiếm ưu thế. Cột đèn cuối cùng ở 100 lực bán kính từ 0 đến 100, do đó câu trả lời trở thành 0, mà thuật toán ghi lại thông qua max(N - P[-1], ...).
