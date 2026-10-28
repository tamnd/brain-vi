---
title: "CF 106151B - phá vỡ thực phẩm"
description: "Chúng ta có một ngày bắt đầu tại thời điểm 0 và kết thúc tại thời điểm T. Trong khoảng thời gian này, có N bản trình bày được lên lịch, mỗi bản trình bày chiếm một phân đoạn nửa mở [si, ei), nghĩa là nó bắt đầu tại si và kết thúc ngay trước ei. Những bài thuyết trình này có thể chồng lên nhau hoặc rời rạc."
date: "2026-06-19T19:22:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "B"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 47
verified: true
draft: false
---

[CF 106151B - phá vỡ thực phẩm](https://codeforces.com/problemset/problem/106151/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ngày bắt đầu tại thời điểm 0 và kết thúc tại thời điểm T. Trong khoảng thời gian này, có N bản trình bày được lên lịch, mỗi bản trình bày chiếm một phân đoạn nửa mở [si, ei), nghĩa là nó bắt đầu tại si và kết thúc ngay trước ei. Những bài thuyết trình này có thể chồng lên nhau hoặc rời rạc. 

Giữa các phân đoạn được lên lịch này, có những khoảng thời gian trống không chạy bản trình bày nào. Nhiệm vụ là tìm khoảng thời gian rảnh liên tục dài nhất bên trong [0, T) không được bao phủ bởi bất kỳ bản trình bày nào. 

Chi tiết quan trọng là “khoảng trống” có nghĩa là khoảng thời gian liền kề tối đa không bị trùng lặp với bất kỳ bản trình bày nào. Vì vậy, ngay cả khi tồn tại nhiều khoảng trống nhỏ, chúng tôi chỉ quan tâm đến khoảng thời gian lớn nhất sau khi xem xét tất cả các phần trùng lặp và hợp nhất thời gian chiếm dụng một cách chính xác. 

Các ràng buộc N lên tới 100000 và T lên tới 10^9 ngay lập tức loại trừ mọi phương pháp kiểm tra mọi đơn vị thời gian. Ngay cả việc quét dòng thời gian hoặc thực hiện mô phỏng theo thời gian cũng sẽ quá chậm. Bất kỳ giải pháp đúng nào cũng phải giảm vấn đề xuống việc sắp xếp và xử lý tuyến tính theo từng khoảng thời gian. 

Trường hợp lỗi phổ biến xuất hiện khi các khoảng chồng chéo và không được hợp nhất. Ví dụ: nếu chúng ta có [5, 10] và [8, 12], khoảng trống giữa chúng không nằm trong khoảng từ 10 đến 8, vì các khoảng chồng lên nhau và tạo thành một vùng bị chặn liên tục một cách hiệu quả [5, 12). Một tính toán khoảng cách đơn giản so sánh các khoảng đầu vào liên tiếp mà không chuẩn hóa sẽ tính không chính xác các phần chồng lấp bên trong là không gian trống. 

Một trường hợp khó khăn khác là khi không có bài thuyết trình nào tồn tại. Khi đó toàn bộ khoảng [0, T) là một khoảng trống có độ dài T. Một giải pháp ngây thơ giả sử ít nhất một khoảng sẽ thất bại ở đây. 

Cuối cùng, bản trình bày có thể không được sắp xếp theo thời gian bắt đầu, do đó việc dựa vào thứ tự đầu vào mà không sắp xếp có thể tạo ra các phép tính khoảng cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xây dựng lại toàn bộ dòng thời gian và đánh dấu tất cả các khu vực bị chiếm đóng, sau đó quét các phân đoạn trống. Một cách là sắp xếp các sự kiện trên một đơn vị thời gian hoặc mô phỏng từng khoảng thời gian bắt đầu và kết thúc, nhưng vì T có thể lớn tới 10^9 nên mọi sự rời rạc hóa theo thời gian là không thể. Ngay cả khi chúng tôi cố gắng lặp lại tất cả các số nguyên có thể, thì đó sẽ là 10^9 phép toán, vượt xa giới hạn. 

Một cách tiếp cận bạo lực có cấu trúc hơn là sắp xếp các khoảng và sau đó, với mỗi khoảng, liên tục hợp nhất nó thành một tập hợp chiếm chỗ ngày càng tăng. Khi chúng ta có sự kết hợp của tất cả các khoảng bị chiếm dụng, chúng ta có thể quét giữa chúng để tính toán các khoảng trống. Phiên bản đơn giản vẫn có thể tính toán lại các phần chồng lấp không hiệu quả nếu được thực hiện bằng cách quét lặp lại, có khả năng dẫn đến hành vi O(N^2). 

Nhận xét quan trọng là tất cả những gì chúng ta thực sự cần là sự kết hợp của các khoảng. Khi các khoảng thời gian được sắp xếp theo thời gian bắt đầu, việc hợp nhất chúng một cách tham lam trong một lần sẽ tạo ra các phân đoạn bị chiếm đóng rời rạc. Sau đó, khoảng trống chỉ đơn giản là sự khác biệt giữa các phân đoạn được hợp nhất liên tiếp, cộng với ranh giới tại 0 và T. 

Cấu trúc cho phép điều này là sự kết hợp khoảng trên một dòng được xác định đầy đủ bằng cách sắp xếp các điểm cuối và quét một lần. Không cần thêm lý do toàn cầu nào ngoài việc duy trì phân khúc đã hợp nhất hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T + N^2) hoặc O(T) | O(T) | Quá chậm | 
| Tối ưu (sắp xếp + hợp nhất) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp tất cả các bài thuyết trình theo thời gian bắt đầu. Điều này đảm bảo chúng tôi xử lý các khoảng thời gian theo thứ tự thời gian, do đó, sự trùng lặp trở nên dễ dàng được phát hiện. 
2. Khởi tạo biến current_end thành 0, biểu thị phần cuối của phân đoạn bị chiếm đóng được hợp nhất cuối cùng. 
3. Khởi tạo best_gap thành 0, nó sẽ theo dõi thời gian rảnh tối đa được tìm thấy cho đến nay. 
4. Lặp lại từng khoảng [s, e] theo thứ tự đã sắp xếp. 
5. Nếu s lớn hơn current_end thì có một khoảng cách trống giữa current_end và s. Cập nhật best_gap với s - current_end. Điều này hợp lệ vì tất cả các khoảng thời gian trước đó đều kết thúc không muộn hơn current_end, vì vậy không có gì chặn vùng này. 
6. Cập nhật current_end thành max(current_end, e). Điều này hợp nhất các khoảng thời gian chồng chéo thành một phân đoạn được sử dụng duy nhất. 
7. Sau khi xử lý tất cả các khoảng thời gian, có thể vẫn còn thời gian rảnh giữa current_end và T. Cập nhật best_gap bằng T - current_end. 

Lý do nó hoạt động là sau khi sắp xếp, chúng tôi duy trì một bất biến mà current_end luôn đại diện cho điểm xa nhất được bao phủ bởi sự kết hợp của tất cả các khoảng được xử lý. Bất cứ khi nào chúng tôi gặp thời gian bắt đầu ngoài thời điểm này, chúng tôi đã khám phá đầy đủ phân đoạn miễn phí tối đa. Vì mỗi khoảng thời gian được xử lý một lần theo thứ tự và được hợp nhất một cách tham lam nên không có vùng bị chiếm nào bị bỏ sót và không có khoảng trống nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, T = map(int, input().split())
    intervals = []

    for _ in range(n):
        s, e = map(int, input().split())
        intervals.append((s, e))

    intervals.sort()

    current_end = 0
    best_gap = 0

    for s, e in intervals:
        if s > current_end:
            best_gap = max(best_gap, s - current_end)
        current_end = max(current_end, e)

    best_gap = max(best_gap, T - current_end)

    print(best_gap)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc tất cả các khoảng và sắp xếp chúng sao cho các phần chồng chéo trở nên liền kề nhau theo thứ tự xử lý. Biến current_end theo dõi thời gian xa nhất được bao gồm trong các bản trình bày được hợp nhất cho đến nay. Bất cứ khi nào chúng tôi thấy một khoảng mới bắt đầu sau current_end, chúng tôi sẽ đo ngay khoảng trống giữa chúng. Cập nhật current_end bằng max đảm bảo các khoảng chồng chéo hoặc chạm được hợp nhất chính xác thay vì được coi là các khối riêng biệt. Bản cập nhật cuối cùng chống lại T nắm bắt được khoảng trống sau phần trình bày cuối cùng. 

Một điểm tinh tế là sử dụng phép so sánh chặt chẽ s > current_end thay vì s >= current_end. Sử dụng s >= current_end cũng đúng trong vấn đề này vì việc chạm vào các khoảng như [1,2) và [2,3) không tạo ra khoảng cách. Tuy nhiên, việc sử dụng s > current_end làm cho ý định rõ ràng rằng chỉ có thời gian được phát hiện nghiêm ngặt mới góp phần tạo ra khoảng trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

4 60 

5 10 

50 55 

25 30 

15 20 

Khoảng thời gian được sắp xếp trở thành: 

[5,10], [15,20], [25,30], [50,55] 

| Khoảng thời gian | current_end trước | Khoảng trống được phát hiện | current_end sau | tốt nhất_gap | 
| --- | --- | --- | --- | --- | 
| [5,10] | 0 | 5 | 10 | 5 | 
| [15,20] | 10 | 5 | 20 | 5 | 
| [25,30] | 20 | 5 | 30 | 5 | 
| [50,55] | 30 | 20 | 55 | 20 | 

Sau vòng lặp, khoảng cách cuối cùng là 60 - 55 = 5, vậy đáp án là 20. 

Dấu vết này cho thấy cách thuật toán chỉ tích lũy các khoảng trống giữa các phân đoạn được sử dụng đã hợp nhất chứ không phải giữa các khoảng thời gian đầu vào thô. 

### Ví dụ 2 

đầu vào: 

3 10 

2 3 

3 5 

6 8 

Các khoảng được sắp xếp: 

[2,3], [3,5], [6,8] 

| Khoảng thời gian | current_end trước | Khoảng trống được phát hiện | current_end sau | tốt nhất_gap | 
| --- | --- | --- | --- | --- | 
| [2,3] | 0 | 2 | 3 | 2 | 
| [3,5] | 3 | 0 | 5 | 2 | 
| [6,8] | 5 | 1 | 8 | 2 | 

Khoảng cách cuối cùng là 10 - 8 = 2, đáp án là 2. 

Ví dụ này nhấn mạnh rằng các khoảng chạm không tạo ra khoảng trống và việc hợp nhất sẽ ngăn chặn các khoảng trống nội bộ giả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(N) | Lưu trữ trong khoảng thời gian | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì N lên tới 100000 và việc sắp xếp cộng với một lần vượt qua cũng nằm trong giới hạn thời gian trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()

    def solve():
        n, T = map(int, sys.stdin.readline().split())
        intervals = []
        for _ in range(n):
            s, e = map(int, sys.stdin.readline().split())
            intervals.append((s, e))

        intervals.sort()
        current_end = 0
        best_gap = 0

        for s, e in intervals:
            if s > current_end:
                best_gap = max(best_gap, s - current_end)
            current_end = max(current_end, e)

        best_gap = max(best_gap, T - current_end)
        print(best_gap)

    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""4 60
5 10
50 55
25 30
15 20
""") == "20"

# no presentations
assert run("""0 10
""") == "10"

# fully covered
assert run("""2 5
0 3
3 5
""") == "0"

# overlapping intervals
assert run("""3 10
1 5
2 6
7 9
""") == "1"

# disjoint with boundary gaps
assert run("""2 10
2 3
8 9
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lịch trống | 10 | xử lý khoảng trống cả ngày | 
| bảo hiểm đầy đủ | 0 | trường hợp không có khoảng cách | 
| hợp nhất chồng chéo | 1 | sáp nhập công đoàn đúng | 
| khoảng trống ranh giới | 5 | tính chính xác của khoảng cách tiền tố/hậu tố | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi không có khoảng thời gian nào cả. Thuật toán không bao giờ đi vào vòng lặp, current_end vẫn bằng 0 và câu trả lời cuối cùng trở thành T - 0, tạo ra khoảng cách cả ngày một cách chính xác. 

Một trường hợp khác là phạm vi bao phủ hoàn chỉnh trong đó các khoảng hợp nhất mở rộng đến T. Đối với đầu vào như [0,3] và [3,5], current_end trở thành 5 và khoảng cách cuối cùng là T - current_end = 0, phản ánh chính xác không có thời gian rảnh. 

Một trường hợp tinh tế là các chuỗi chồng chéo trong đó các khoảng không chồng chéo trực tiếp theo cặp nhưng vẫn tạo thành một khối lớn. Ví dụ [1,4], [3,6], [5,10]. Quá trình quét đảm bảo current_end tăng đều đặn lên 10, ngăn chặn bất kỳ khoảng cách sai nào giữa các điểm cuối trung gian.
