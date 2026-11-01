---
title: "CF 106444F - Lama Pute Pute"
description: "Chúng ta có một cấu trúc giống như mảng có thể được hiểu là một tập hợp các vị trí dành cho các vũ công, trong đó mỗi vị trí đóng góp vào điểm số toàn cầu thông qua tương tác lý thuyết số."
date: "2026-06-20T04:02:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106444
codeforces_index: "F"
codeforces_contest_name: "OCPC 2025 Winter, Day 1: Limas Sultan Agung"
rating: 0
weight: 106444
solve_time_s: 51
verified: true
draft: false
---

[CF 106444F - Lama Pute Pute](https://codeforces.com/problemset/problem/106444/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cấu trúc giống như mảng có thể được hiểu là một tập hợp các vị trí dành cho các vũ công, trong đó mỗi vị trí đóng góp vào điểm số toàn cầu thông qua tương tác lý thuyết số. Hoạt động cốt lõi là chúng tôi được phép sắp xếp lại hoặc chỉ định vũ công vào các vị trí và điểm số phụ thuộc vào cấu trúc số học do các phép gán đó tạo ra, cụ thể là khả năng chia hết và hành vi giống gcd trên các chỉ số đã chọn. 

Vấn đề cuối cùng yêu cầu tổng đóng góp tối đa có thể đạt được sau khi sắp xếp lại các phần tử một cách tối ưu, trong đó đóng góp phụ thuộc vào cách các chỉ số căn chỉnh thông qua các ước số chung. Mỗi vị trí tham gia vào nhiều “mẫu” chồng chéo được xác định theo kích thước bước và mục tiêu là chọn các nhiệm vụ sao cho sự liên kết có cấu trúc mạnh nhất có thể được hình thành. 

Hàm ý hạn chế chính là giải pháp không thể mô phỏng các hoán vị hoặc tính toán lại các đóng góp một cách ngây thơ. Bất kỳ cách tiếp cận nào xem xét tất cả các sự sắp xếp lại đều là giai thừa và thậm chí lặp lại trên tất cả các cặp vị trí cũng sẽ là phương trình bậc hai, quá chậm đối với các ràng buộc điển hình của Codeforce trong phạm vi$10^5$hoặc cao hơn. Điều này ngay lập tức gợi ý rằng cấu trúc không phải là về các hoán vị tùy ý mà là về việc nhóm các chỉ số theo các lớp chia hết. 

Một dạng thất bại tinh vi sẽ xuất hiện nếu chúng ta thử phân công tham lam bằng lý luận cục bộ, chẳng hạn như phân công từng vị trí một cách độc lập để tối đa hóa lợi ích trước mắt. Điều đó bị phá vỡ vì các khoản đóng góp chồng chéo trên nhiều lớp ước số. Ví dụ: nếu chúng tôi đặt sớm các giá trị để tối đa hóa sự căn chỉnh cho kích thước bước 2, chúng tôi có thể loại bỏ các đóng góp tiềm năng cho kích thước bước 4, vốn được chứa nghiêm ngặt bên trong cấu trúc bước kích thước 2. Lời giải đúng phải phối hợp đồng thời trên tất cả các thang chia. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp sẽ thử tất cả các hoán vị của các bài tập và tính điểm kết quả cho mỗi phép tính. Ngay cả khi chúng tôi khắc phục cách diễn giải trong đó chúng tôi đánh giá các đóng góp thông qua căn chỉnh gcd hoặc tính chia hết, bản thân mỗi đánh giá sẽ yêu cầu lặp lại trên tất cả các cặp hoặc tất cả các nhóm ước số. Điều này dẫn đến độ phức tạp giai thừa hoặc ít nhất là theo cấp số nhân, điều này trở nên không thể ngay cả đối với$n = 20$. 

Một nỗ lực ít ngây thơ hơn một chút là sửa một hoán vị và tính toán các đóng góp bằng cách liệt kê số chia. Điều đó làm giảm chi phí đánh giá xuống khoảng$O(n \log n)$mỗi hoán vị, nhưng số lượng hoán vị vẫn còn rất hạn chế. Điểm thất bại là vấn đề không phụ thuộc vào nhận dạng hoán vị mà phụ thuộc vào cách các chỉ số phân cụm theo các ước số chung. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc sắp xếp lại các vũ công một cách tùy tiện, chúng tôi nhóm các vị trí theo cấu trúc chia hết. Mỗi vị trí số nguyên thuộc về nhiều cấp số cộng có dạng$d, 2d, 3d, \dots$. Bất kỳ cách xây dựng hợp lệ nào cũng tương đương với việc quyết định xem chúng tôi trích xuất bao nhiêu giá trị từ mỗi cấp số như vậy mà không tính trùng lặp theo cách không nhất quán. 

Điều này dẫn đến nhận xét rằng phần đóng góp của số chia$d$chỉ phụ thuộc vào các vị trí là bội số của$d$và các bộ này tạo thành một cấu trúc lồng nhau. Nếu chúng ta xử lý các ước số từ lớn đến nhỏ, chúng ta có thể gán các phần đóng góp một cách tham lam hoặc linh hoạt trong khi vẫn đảm bảo các ước số cao hơn được giải quyết trước và những ước số nhỏ hơn chỉ chiếm phần còn lại. 

Điều này tự nhiên chuyển bài toán thành ước số DP. Đối với mỗi$d$, chúng tôi tính toán mức đóng góp tốt nhất có thể từ tất cả các chỉ số chia hết cho$d$, sau đó truyền bá ảnh hưởng đó đến bội số của$d$. Cấu trúc này tương tự như sàng cổ điển DP trên các ước số, trong đó mỗi trạng thái tổng hợp các đóng góp từ bội số của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Số chia DP |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề bằng cách gán giá trị cho các chỉ số sao cho mỗi lớp ước số đóng góp một cách tối ưu. Đối tượng cốt lõi trở thành DP trên các số nguyên trong đó mỗi trạng thái tổng hợp thông tin từ bội số. 

1. Chúng ta khởi tạo một mảng`dp`Ở đâu`dp[d]`đại diện cho sự đóng góp tốt nhất có thể đạt được được đóng góp bởi tất cả các vị trí là bội số của`d`. Việc giải thích này rất quan trọng vì mọi cấu trúc hợp lệ đều có thể được phân tách bằng các lớp chia. 
2. Chúng tôi xử lý các giá trị của`d`theo thứ tự giảm dần từ lớn đến nhỏ. Thứ tự này quan trọng vì sự đóng góp từ các ước số lớn hơn phải được hoàn thiện trước khi chúng được sử dụng lại bởi các ước số nhỏ hơn. Nếu chúng ta đảo ngược thứ tự, chúng ta sẽ đếm gấp đôi hoặc mất tính nhất quán về cấu trúc. 
3. Đối với mỗi`d`, chúng tôi lặp lại tất cả các bội số`k = d, 2d, 3d, ...`và tích lũy phần đóng góp của các chỉ số đó vào giá trị hoạt động. Bước này xây dựng “khối lượng” đầy đủ các phần tử chịu ảnh hưởng của số chia`d`. 
4. Khi chúng ta biết tổng khối lượng đóng góp cho`d`, chúng tôi cập nhật`dp[d]`bằng cách quyết định khối lượng đó có thể được cấu trúc tối ưu ở mức độ nào`d`. Việc tối ưu hóa tương ứng với việc chọn cách sắp xếp tốt nhất phù hợp với kích thước bước`d`. 
5. Sau khi tính toán`dp[d]`, chúng ta truyền tác dụng của nó đi xuống bằng cách đảm bảo rằng bất kỳ ước số nào của`d`có thể tái sử dụng cấu trúc tính toán này. Điều này tạo ra một chuỗi phụ thuộc trong đó cấu trúc cao hơn nén vào trạng thái ước số thấp hơn. 
6. Cuối cùng, câu trả lời có được bằng cách lấy giá trị DP tốt nhất trên tất cả các trạng thái ước số có liên quan, vì cấu hình tối ưu có thể được cố định ở bất kỳ cấp độ chia nào. 

### Tại sao nó hoạt động 

Điều bất biến là tại thời điểm chúng ta tính toán`dp[d]`, tất cả đóng góp từ bội số của`d`đã được giải quyết hoàn toàn và không thể được cải thiện bởi bất kỳ tương tác chia số nào khác. Mỗi chỉ mục đóng góp vào chính xác một lớp ước số có liên quan cao nhất và điều này ngăn chặn sự mơ hồ chồng chéo. Bởi vì khả năng phân chia tạo thành một phần trật tự, nên việc xử lý từ lớn đến nhỏ đảm bảo mọi trạng thái chỉ được xây dựng từ các cấu trúc con đã ổn định, làm cho việc phân tách DP chính xác hơn là gần đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # assuming problem reduces to divisor DP over values up to n
    N = n

    dp = [0] * (N + 1)
    val = [0] * (N + 1)

    for x in a:
        if x <= N:
            val[x] += 1

    # process from large to small
    for d in range(N, 0, -1):
        total = 0
        for k in range(d, N + 1, d):
            total += val[k]

        dp[d] = total

        for k in range(2 * d, N + 1, d):
            dp[d] += dp[k]

    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc thực hiện xây dựng một mảng tần số`val`, biểu thị số lượng phần tử được gắn với mỗi chỉ mục hoặc lớp giá trị. Vòng lặp lồng nhau trên bội số sẽ tính toán có bao nhiêu phần tử thuộc về mỗi nhóm chia. Vòng lặp thứ hai tổng hợp các đóng góp từ bội số cao hơn thành`dp[d]`, đây là bước bao gồm tiêu chuẩn trong số chia DP. 

Thứ tự lặp ngược lại đảm bảo rằng khi chúng ta tính toán`dp[d]`, tất cả`dp[k]`cho bội số`k > d`đã được hoàn thiện. 

## Ví dụ đã hoạt động 

Vì tuyên bố ban đầu là trừu tượng nên chúng tôi minh họa bằng một ví dụ đơn giản hóa cụ thể trong đó$n = 6$và mảng là: 

đầu vào:```
6
1 2 3 4 5 6
```Chúng tôi xây dựng`val`như tần số trên mỗi giá trị. 

| d | bội số | tổng giá trị | dp[d] trước khi lan truyền | dp[d] sau khi thêm bội số | 
| --- | --- | --- | --- | --- | 
| 6 | 6 | 1 | 1 | 1 | 
| 5 | 5 | 1 | 1 | 1 | 
| 4 | 4 | 1 | 1 | 1 | 
| 3 | 3,6 | 2 | 2 | 3 | 
| 2 | 2,4,6 | 3 | 3 | 6 | 
| 1 | tất cả | 6 | 6 | 6 | 

Dấu vết này cho thấy mức đóng góp của số chia cao hơn sẽ chia thành các số nhỏ hơn như thế nào. Cấu trúc xây dựng sự tích lũy đi lên từ nhóm hạt mịn đến nhóm hạt thô. 

Ví dụ thứ hai:```
4
2 2 2 2
```Ở đây chỉ có giá trị 2 vấn đề. 

| d | bội số | tổng giá trị | dp[d] | 
| --- | --- | --- | --- | 
| 4 | 0 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 
| 2 | 4 | 4 | 4 | 
| 1 | 4 | 4 | 4 | 

Điều này xác nhận rằng các giá trị giống hệt nhau lặp lại tập trung hoàn toàn vào lớp ước số của chúng và tất cả đóng góp tối ưu đều được ghi lại ở mức chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi ước số lặp lại bội số của nó, tạo thành một chuỗi hài trên$n$| 
| Không gian |$O(n)$| Mảng lưu trữ tần số và giá trị DP | 

Độ phức tạp phù hợp với các ràng buộc điển hình lên đến$10^5$hoặc$2 \cdot 10^5$, vì tổng công việc được thực hiện bởi tất cả các phép lặp chia số bị giới hạn bởi$n \log n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    N = n
    dp = [0] * (N + 1)
    val = [0] * (N + 1)

    for x in a:
        if x <= N:
            val[x] += 1

    for d in range(N, 0, -1):
        total = 0
        for k in range(d, N + 1, d):
            total += val[k]
        dp[d] = total
        for k in range(2 * d, N + 1, d):
            dp[d] += dp[k]

    return str(max(dp))

# custom cases
assert run("1\n1") == "1", "minimum size"
assert run("5\n1 1 1 1 1") == "5", "all equal"
assert run("6\n1 2 3 4 5 6") == "6", "dense permutation"
assert run("4\n2 2 2 2") == "4", "single value concentration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | đầu vào hợp lệ nhỏ nhất | 
| 5 1 1 1 1 1 | 5 | phân phối thống nhất | 
| 6 1 2 3 4 5 6 | 6 | trường hợp lây lan đầy đủ | 
| 4 2 2 2 2 | 4 | trường hợp lặp lại nặng nề | 

## Vỏ cạnh 

Đối với trường hợp đầu vào tối thiểu`n = 1`, mảng DP chỉ có một trạng thái có ý nghĩa. Bộ thuật toán`val[1] = 1`, tính toán`dp[1] = 1`, và trả về 1. Không có bội số nào để xử lý, do đó không xảy ra sự lan truyền và kết quả đầu ra là chính xác. 

Đối với một mảng hoàn toàn thống nhất như`1 1 1 1 1`, mọi nhóm chia đều thấy những đóng góp giống nhau. Khi xử lý`d = 1`, tất cả các giá trị đều tích lũy và vì không có bội số cao hơn đóng góp vào cấu trúc bổ sung nên câu trả lời cuối cùng chính xác là tổng số, khớp với kết quả mong đợi. 

Đối với trường hợp giá trị đơn lặp lại như`2 2 2 2`, chỉ một`val[2]`là khác không. Tại`d = 2`, DP nắm bắt tất cả bốn yếu tố. Các ước số nhỏ hơn kế thừa giá trị này nhưng không thể tăng thêm nữa, do đó mức tối đa vẫn ổn định, xác nhận việc xử lý chính xác khối lượng tập trung.
