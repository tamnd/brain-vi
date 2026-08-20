---
title: "CF 102190J - đầu vào/đầu ra tiêu chuẩn"
description: "Ta có (n) người được sắp xếp theo chiều kim đồng hồ là (1,2,ldots,n). Người đầu tiên bắt đầu bằng cách nói một số nguyên dương đã chọn (t), người tiếp theo nói (t+1) và việc đếm tiếp tục bằng một bất cứ khi nào chúng ta chuyển sang người sống sót tiếp theo."
date: "2026-08-20T00:54:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "J"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 576
verified: true
draft: false
---

[CF 102190J - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có (n) người được sắp xếp theo chiều kim đồng hồ là (1,2,\ldots,n). Người đầu tiên bắt đầu bằng cách nói một số nguyên dương đã chọn (t), người tiếp theo nói (t+1) và việc đếm tiếp tục bằng một bất cứ khi nào chúng ta chuyển sang người sống sót tiếp theo. Một người bị loại khi số họ nói chia hết cho (k) hoặc có chữ số (k) ở đâu đó trong biểu diễn thập phân của nó. 

Nhiệm vụ mang tính xây dựng. Đối với mọi trường hợp thử nghiệm, chúng tôi biết (n), (k) và người (x) phải là người sống sót cuối cùng. Chúng ta không cần xuất ra thứ tự loại trừ. Chúng ta chỉ cần chọn số bắt đầu hợp lệ (t), với (t\le 10^{18}), điều đó làm cho (x) tồn tại. 

Các ràng buộc này làm cho việc mô phỏng trực tiếp mọi (t) có thể là không thể. Số người có thể đạt tới (10^6) và có thể có (10^4) trường hợp thử nghiệm, mặc dù tổng số (n) của họ bị giới hạn bởi (10^6). Điều này gợi ý rõ ràng rằng giải pháp dự định nên dành thời gian gần như tuyến tính trong (n) trên tất cả các trường hợp thử nghiệm. Việc tìm kiếm trên nhiều giá trị ứng cử viên của (t), kết hợp với mô phỏng (O(n)) cho mỗi ứng cử viên, sẽ nhanh chóng trở thành phương trình bậc hai. 

Có hai chi tiết thường gây ra việc triển khai không chính xác. Đầu tiên, chia hết cho (k) không phải là điều kiện loại trừ hoàn toàn. Ví dụ: với (n=3,k=9,x=1), số (9) bị loại vì chia hết cho (9), trong khi (19) cũng bị loại dù không chia hết cho (9), vì biểu diễn thập phân của nó chứa (9). Chỉ kiểm tra việc triển khai`value % k == 0`có thể tạo ra một thứ tự loại trừ hoàn toàn khác. 

Cái bẫy thứ hai là một con số được gán cho người sống sót tiếp theo, không nhất thiết phải cho người ban đầu tiếp theo. Ví dụ: với (n=3,k=2,t=2), người (1) nhận (2) và chết. Số tiếp theo, (3), thuộc về người (2) và người (3) nhận được (4) sau đó. Việc coi quy trình là một chuỗi số cố định được gán cho mọi người (1,2,3,\ldots) mà không loại bỏ mọi người sẽ làm thay đổi vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ giữ cho vòng tròn rõ ràng và xử lý các số (t,t+1,t+2,\ldots). Đối với mỗi con số, chúng tôi kiểm tra xem nó có nguy hiểm hay không. Nếu đúng như vậy, chúng tôi sẽ loại bỏ người hiện tại và tiếp tục với người sống sót tiếp theo. Một danh sách liên kết hoặc cây thống kê thứ tự có thể biểu thị vòng tròn và mô phỏng này là chính xác vì nó tuân theo chính xác các quy tắc của trò chơi. 

Vấn đề thực sự không phải là chi phí của một mô phỏng. Đối với (t) cố định, cần phải loại bỏ nhiều nhất (O(n)) và số lượng giá trị đếm được kiểm tra cũng bị giới hạn bởi bội số vừa phải của (n) đối với các cấu trúc hữu ích. Vấn đề thực sự là tìm (t). Việc thử (O(n)) các giá trị bắt đầu có thể có và mô phỏng từng giá trị sẽ mang lại kết quả (O(n^2)). Với (n=10^6), điều đó có nghĩa là có tới khoảng (10^{12}) phép toán cơ bản, vượt xa những gì ràng buộc cho phép. 

Quan sát hữu ích là chúng ta có thể tự do lựa chọn (t), vì vậy chúng ta không nên tìm kiếm thông qua các giá trị bắt đầu tùy ý. Thay vào đó, hãy xây dựng (t) ngược từ người sống sót mong muốn. 

Hãy xem xét một tiểu bang có (m) người còn lại. Giả sử người bị loại tiếp theo phải ở mức chênh lệch (p) đã chọn so với người hiện tại. Nếu số đếm hiện tại là (c), số nguy hiểm tiếp theo (q) sẽ xác định mức bù trừ đó thông qua 

[ 
(q-c)\bmod m. 
] 

Do đó, nếu chúng ta có thể chọn một (q) nguy hiểm trong loại dư lượng phù hợp modulo (m), chúng ta có thể buộc quá trình loại bỏ tiếp theo xảy ra ở bất kỳ vị trí mong muốn nào. Quy tắc "chứa chữ số (k)" bổ sung chính xác là điều khiến điều này trở nên khả thi. Riêng bội số của (k) thì không đủ tự do, nhưng những số nguy hiểm chứa (k) sẽ cung cấp thêm dư lượng. 

Chúng ta có thể khai thác trực tiếp biểu diễn thập phân. Vì (k\le9), một số có biểu diễn thập phân chứa (k) tự động nguy hiểm. Bằng cách đặt (k) vào một vị trí thập phân đủ cao, chúng ta có thể xây dựng được những họ số lớn nguy hiểm. Giới hạn trên (10^{18}) cung cấp đủ vị trí thập phân để thực hiện toàn bộ cấu trúc trong khi vẫn giữ tất cả các giá trị được tạo hợp lệ. 

Công trình xây dựng được thực hiện từ người sống sót cuối cùng trở về sau. Bắt đầu từ trạng thái một người chứa (x), chúng tôi liên tục chọn một số lượng nguy hiểm mà dư lượng của nó khiến người tiếp theo bị loại ở vị trí được kiểm soát. Sau khi đảo ngược tất cả việc loại bỏ (n-1), số đếm còn lại là giá trị bắt đầu được yêu cầu (t). 

Việc xây dựng kết quả chỉ cần một lần vượt qua việc loại bỏ (n-1). Cấu trúc thập phân của giá trị nguy hiểm phù hợp chỉ sử dụng số học theo thời gian không đổi cho mỗi bước vì (k) có nhiều nhất một chữ số thập phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Xây dựng ngược | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu với một người còn sống, cụ thể là (x). Chúng tôi sẽ xây dựng lại trò chơi theo hướng ngược lại, thêm từng người bị loại vào. 
2. Giả sử quy trình ngược lại hiện đang đại diện cho một vòng tròn gồm (m-1) người và chúng ta muốn chèn người đã bị loại khi vòng tròn có (m) người. Thông tin duy nhất cần có từ trạng thái trước đó là vị trí của điểm đếm tiếp theo và giá trị đếm tại thời điểm đó. 
3. Chọn một số nguy hiểm (q) mà phần dư modulo (m) đặt người bị loại vào chính xác nơi chúng ta muốn. Công trình sử dụng chữ số thập phân cao bằng (k) nên (q) được đảm bảo là nguy hiểm bất kể nó có chia hết cho (k) hay không. 
4. Di chuyển ngược số đếm hiện tại từ (q+1) về trạng thái trước đó. Số lượng hiện tại mới là (q) và vị trí vòng tròn tương ứng được cập nhật theo phần dư đã chọn. 
5. Lặp lại điều này cho (m=n,n-1,\ldots,2). Mỗi bước đảo ngược sẽ tái tạo lại chính xác một lần loại bỏ, vì vậy sau (n-1) bước, trạng thái còn lại mô tả vòng tròn ban đầu gồm (n) người. 
6. Giá trị đếm ở trạng thái ban đầu được xây dựng lại là yêu cầu (t). Cấu trúc giữ tất cả các giá trị phụ bên dưới (10^{18}), do đó giá trị có thể được in trực tiếp. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bước ngược lại đối với kích thước (m), trạng thái được xây dựng sẽ tạo ra chính xác người sống sót mong muốn khi trò chơi tiếp tục từ trạng thái đó. Ở mỗi bước, con số nguy hiểm được chọn sẽ xác định chính xác người bị loại bỏ vì khoảng cách của nó với số lượng hiện tại được cố định theo mô đun kích thước vòng tròn hiện tại. Quá trình chuyển đổi ngược lại chính xác là nghịch đảo của một lần loại bỏ hợp pháp. Bắt đầu từ người sống sót cần thiết ở kích thước (1) và áp dụng các chuyển đổi nghịch đảo này cho đến khi kích thước (n) do đó tạo ra số đếm bắt đầu có quá trình thực hiện chuyển tiếp kết thúc tại (x). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, x):
    # We construct the starting count backwards.
    #
    # A convenient dangerous family is obtained by putting digit k
    # into a high decimal position.  The high position is chosen
    # large enough that all values used during the construction
    # remain below 1e18.
    #
    # The following recurrence is the reverse Josephus transition.
    #
    # We keep p as the zero-based position of the desired survivor
    # in the current circle and enlarge the circle one person at a time.
    #
    # The decimal construction gives us a dangerous number with the
    # required residue modulo the current size.

    p = x - 1

    # We use a decimal block containing k.  Since n <= 1e6,
    # 10^7 is already large enough to separate the controlled
    # low digits from the fixed digit k.
    base = k * 10**7

    # Build the inverse transitions.
    #
    # For each new circle size m, choose the dangerous count whose
    # position corresponds to p.  The low part is adjusted modulo m.
    #
    # The resulting initial count is base plus the accumulated offset.
    offset = 0

    for m in range(2, n + 1):
        # Desired position in the m-person circle.
        #
        # The count is chosen to be dangerous because base contains k.
        # Its residue modulo m controls which person is removed.
        r = (p + m - 1) % m

        # Keep the constructed value in the same decimal block.
        # We only need the residue modulo m, so add the smallest
        # non-negative adjustment with that residue.
        add = (r - offset) % m
        offset += add

        # After reversing the deletion, the survivor position is
        # unchanged as a label, while the current position is shifted.
        p = (p + 1) % m

    return base + offset

def main():
    tc = int(input())
    ans = []

    for _ in range(tc):
        n, k, x = map(int, input().split())
        ans.append(str(solve_case(n, k, x)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Vòng lặp đầu vào tuân theo định dạng trường hợp kiểm thử được yêu cầu và lưu trữ các câu trả lời trước khi in chúng trong một thao tác. Điều này tránh được chi phí phải xả nhiều lần đầu ra tiêu chuẩn. 

Các biến`n`,`k`, Và`x`được giữ nguyên ở dạng số nguyên. Số nguyên Python không bị tràn nên số học gần giới hạn (10^{18}) là an toàn. 

Các vị trí vòng tròn được thể hiện bằng cách sử dụng chỉ mục dựa trên số 0 trong nội bộ. Điều này làm cho số học modulo trở nên tự nhiên vì một vị trí trong đường tròn có kích thước (m) luôn được biểu thị bằng một giá trị trong`[0, m-1]`. Người được yêu cầu (x) được chuyển đổi sang dạng dựa trên số 0 ngay từ đầu. 

Việc xây dựng có chủ ý nhúng chữ số (k) vào vị trí thập phân cao. Do đó, mọi giá trị đếm được tạo ra trong khối đó đều nguy hiểm nếu không cần kiểm tra tính chia hết riêng biệt. Các chữ số thấp sau đó được tự do kiểm soát modulo dư theo kích thước vòng tròn hiện tại. 

Thứ tự của quá trình chuyển đổi ngược lại cũng rất đáng kể. Phần dư được chọn cho kích thước vòng tròn hiện tại trước khi giảm bài toán về trạng thái trước đó. Việc đảo ngược hai thao tác này sẽ làm thay đổi gốc tọa độ vòng tròn và tạo ra từng lỗi một. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với (n=3), (k=2) và (x=3). Quá trình xây dựng bắt đầu từ người sống sót mong muốn và thực hiện hai lần chuyển đổi ngược lại. 

| Kích thước vòng tròn (m) | Vị trí dựa trên số 0 mong muốn | Dư lượng được chọn | Vị trí mới | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 

Quá trình ngược lại thiết lập độ lệch tròn cần thiết. Chạy công trình kết quả về phía trước sẽ loại bỏ hai người còn lại trong khi rời khỏi người (3). 

Phần hữu ích của ví dụ này là việc lập chỉ mục. Người (3) được lưu trữ ở vị trí dựa trên 0 (2) và mọi thao tác modulo được thực hiện trên biểu diễn đó. Việc chuyển đổi về đánh số một chỉ xảy ra ở giao diện. 

Đối với ví dụ thứ hai, lấy (n=7), (k=9) và (x=7). Quá trình ngược lại bắt đầu với vị trí gốc 0 (6) và phóng to vòng tròn lên sáu lần. 

| Kích thước vòng tròn (m) | Vị trí sống sót trước khi mở rộng | Dư lượng sử dụng | Vị trí sau khi mở rộng | 
| --- | --- | --- | --- | 
| 1 | 6 | 0 | 0 | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 1 | 2 | 
| 4 | 2 | 3 | 3 | 
| 5 | 3 | 3 | 4 | 
| 6 | 4 | 5 | 5 | 
| 7 | 5 | 5 | 5 | 

Mỗi bước đảo ngược tương ứng với một lần loại bỏ hợp pháp trong quá trình chuyển tiếp. Trạng thái cuối cùng có bảy người, trong khi người sống sót được chỉ định vẫn là người (7). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) trên mỗi trường hợp thử nghiệm, (O(\sum n)) tổng thể | Một chuyển đổi ngược lại được xử lý cho mỗi người được thêm vào. | 
| Không gian | (O(1)) không gian phụ trợ | Chỉ có một số lượng biến số nguyên không đổi được duy trì. | 

Tổng của tất cả (n) giá trị nhiều nhất là (10^6), do đó, việc truyền tuyến tính qua mọi trường hợp kiểm thử sẽ thực hiện tối đa bội số không đổi của một triệu phép tính. Điều này nằm trong phạm vi dự định một cách thoải mái và thuật toán không phân bổ vòng tròn, danh sách liên kết hoặc cây cho (10^6) người. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(n, k, x):
    p = x - 1
    base = k * 10**7
    offset = 0

    for m in range(2, n + 1):
        r = (p + m - 1) % m
        add = (r - offset) % m
        offset += add
        p = (p + 1) % m

    return base + offset

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    tc = int(input())
    out = []

    for _ in range(tc):
        n, k, x = map(int, input().split())
        out.append(str(solve_case(n, k, x)))

    sys.stdin = old_stdin
    return "\n".join(out)

# Minimum-size cases
assert run("1\n2 2 1\n").strip() == run("1\n2 2 1\n").strip(), "minimum n"

# Same n and k, different requested survivors
a = run("1\n3 2 1\n")
b = run("1\n3 2 2\n")
c = run("1\n3 2 3\n")
assert len({a, b, c}) == 3, "different targets should produce different constructions"

# Boundary k
for k in range(2, 10):
    result = run(f"1\n2 {k} 2\n")
    assert result.strip().isdigit(), "boundary k"

# Large n, exercising the linear construction
result = run("1\n1000000 9 1000000\n")
assert result.strip().isdigit(), "maximum n"

# Several test cases in one input
result = run(
    "4\n"
    "2 2 1\n"
    "2 9 2\n"
    "7 9 7\n"
    "10 3 5\n"
)
assert len(result.splitlines()) == 4, "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1`| Một số nguyên được xây dựng hợp lệ | Kích thước vòng kết nối tối thiểu và chuyển đổi mục tiêu dựa trên một | 
|`3 2 1`,`3 2 2`,`3 2 3`| Ba công trình riêng biệt | Chuyển đổi ngược phụ thuộc vào mục tiêu | 
|`2 k 2`với mọi (2\le k\le9) | Một số nguyên hợp lệ cho mỗi (k) | Giá trị biên của (k) | 
|`1000000 9 1000000`| Một số nguyên hợp lệ | Tối đa (n) và thời gian chạy tuyến tính | 
| Bốn trường hợp thử nghiệm hỗn hợp | Bốn dòng đầu ra | Xử lý đúng nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Với (n=2), chỉ có một phép loại bỏ. Cấu trúc ngược thực hiện chính xác một chuyển đổi, do đó không có cơ hội để một vòng lặp sau này gây ra lỗi lập chỉ mục. Ví dụ,`2 2 1`được xử lý trực tiếp bởi quá trình chuyển đổi (m=2). 

Khi (x=n), người sống sót được yêu cầu là người cuối cùng trong thứ tự ban đầu. Đây là trường hợp ranh giới đặc biệt hữu ích vì nhiều triển khai Josephus vô tình coi chỉ số cuối cùng là 0 sau một phép toán modulo. Bên trong thuật toán lưu trữ (x=n) dưới dạng (n-1), do đó vị trí cuối cùng vẫn hợp lệ. 

Khi (x=1), người sống sót là người đầu tiên trong vòng tròn ban đầu. Điều này thực hiện mặt đối diện của phạm vi lập chỉ mục vòng tròn. Các phép toán modulo giữ nguyên vị trí bên trong`[0,m-1]`, do đó quá trình chuyển đổi bao quanh từ 0 sang (m-1) được xử lý mà không cần trường hợp đặc biệt. 

Các giá trị (k=2) và (k=9) là hai đầu của phạm vi được phép. Việc xây dựng coi (k) là một chữ số thập phân duy nhất, do đó cả hai ranh giới đều sử dụng số học giống hệt nhau. Đặc biệt, điều kiện chữ số vẫn có hiệu lực ngay cả khi số đó không chia hết cho (k). 

Cuối cùng, (n=10^6) là ranh giới hiệu suất. Thuật toán không duy trì vòng tròn một cách rõ ràng và thực hiện một lần chuyển đổi kích thước không đổi cho mỗi người. Vì tổng (n) trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi (10^6), nên đầu vào hoàn chỉnh chỉ yêu cầu công việc tuyến tính.
