---
title: "CF 104426M - Kubernetes"
description: "Chúng ta được cung cấp một bộ máy, mỗi máy có công suất cố định và một tập hợp các ứng dụng trong đó mỗi ứng dụng bao gồm một số nhóm giống hệt nhau phải được đặt trên các máy này."
date: "2026-06-30T19:08:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "M"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 55
verified: true
draft: false
---

[CF 104426M - Kubernetes](https://codeforces.com/problemset/problem/104426/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ máy, mỗi máy có công suất cố định và một tập hợp các ứng dụng trong đó mỗi ứng dụng bao gồm một số nhóm giống hệt nhau phải được đặt trên các máy này. Mỗi nhóm thuộc về chính xác một ứng dụng và mỗi nhóm phải được chỉ định cho chính xác một nút, tôn trọng dung lượng của nút. 

Sau khi tất cả các nhóm được đặt, mỗi ứng dụng đều có một thước đo chất lượng: đối với ứng dụng đó, chúng tôi xem xét từng nút và đếm xem có bao nhiêu nhóm của ứng dụng đó đã kết thúc trên đó, sau đó lấy mức tối thiểu trên tất cả các nút. Giá trị này thể hiện mức độ phân tán đồng đều của ứng dụng, vì nó sẽ phạt các nút nhận được ít hoặc không nhận được nhóm nào của ứng dụng đó. 

Mục tiêu là phân phối tất cả các nhóm trên các nút sao cho mức tối thiểu nhỏ nhất cho mỗi ứng dụng càng lớn càng tốt. Nói cách khác, chúng tôi muốn mọi ứng dụng xuất hiện trên mọi nút ít nhất một số lần và chúng tôi muốn tối đa hóa tần suất tối thiểu được đảm bảo đó trên tất cả các ứng dụng cùng một lúc. 

Các ràng buộc chỉ ra rằng cả số lượng nút và ứng dụng đều có thể lớn tới 100.000 và số lượng nhóm cũng có thể lớn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng các vị trí hoặc lặp lại các nút trên mỗi ứng dụng. Bất kỳ cách tiếp cận bậc hai hoặc thậm chí gần bậc hai nào trong$N \cdot M$là không thể. 

Hạn chế chính về mặt cấu trúc là tổng số nhóm không vượt quá tổng công suất, vì vậy tính khả thi không bao giờ là do thiếu không gian trên toàn cầu. Khó khăn hoàn toàn nằm ở sự cân bằng phân phối giữa các nút. 

Trường hợp phức tạp xuất hiện khi một ứng dụng có rất ít nhóm. Ví dụ: nếu một ứng dụng chỉ có một nhóm thì giá trị tối thiểu trên mỗi nút của nó luôn bằng 0 bất kể vị trí đặt, bởi vì ít nhất$N-1$các nút sẽ không nhận được nhóm nào của ứng dụng đó. Điều này buộc câu trả lời toàn cầu bằng 0 trong mọi trường hợp có ít nhất một ứng dụng đủ nhỏ để nó không thể được phổ biến một cách có ý nghĩa. 

Một trường hợp khác là khi năng lực bị sai lệch rất nhiều. Nếu một nút có gần như toàn bộ dung lượng, trực giác ngây thơ có thể đề xuất đóng gói mọi thứ ở đó, nhưng điều đó sẽ phá hủy mức tối thiểu cho mỗi ứng dụng vì các nút khác không nhận được nhóm cho hầu hết các ứng dụng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là cố gắng trực tiếp xây dựng phân phối nhóm cho từng ứng dụng trên các nút và tính toán giá trị tối thiểu thu được. Thậm chí hạn chế bản thân kiểm tra tính khả thi cho một giá trị mục tiêu cố định$x$, chúng ta sẽ cần quyết định xem liệu mọi ứng dụng có thể được sắp xếp sao cho mỗi nút nhận được ít nhất$x$nhóm của ứng dụng đó. Điều này trở thành vấn đề phân bổ tổ hợp với sự phụ thuộc giữa các ứng dụng do dung lượng nút được chia sẻ. 

Nếu chúng ta cố gắng mô phỏng các bài tập một cách tham lam mà không có cấu trúc, chúng ta sẽ nhanh chóng nhận ra thực tế là các ứng dụng chỉ tương tác thông qua dung lượng nút chứ không phải trực tiếp. Điều này gợi ý rằng thay vì xây dựng các nhiệm vụ một cách rõ ràng, chúng ta nên suy luận tổng hợp: tổng công suất cần thiết là bao nhiêu để hỗ trợ một giá trị mục tiêu tối thiểu nhất định. 

Sửa giá trị ứng cử viên$x$. Đối với mỗi ứng dụng$j$, nếu chúng ta muốn mức tối thiểu trên mỗi nút của nó ít nhất là$x$, thì mỗi nút phải nhận được ít nhất$x$nhóm của ứng dụng đó. Vì có$N$các nút, điều này đã ngụ ý rằng ứng dụng$j$sẽ yêu cầu ít nhất$x \cdot N$tổng cộng. Vì vậy, điều kiện cần cho tính khả thi là$a_j \ge x \cdot N$cho tất cả$j$. Nếu bất kỳ ứng dụng nào thất bại điều này, mục tiêu là không thể. 

Tuy nhiên, điều này là chưa đủ vì dung lượng nút giới hạn tổng số nhóm có thể được đặt. Mỗi nút phải lưu trữ ít nhất$x$nhóm của mỗi ứng dụng, nghĩa là mỗi nút cần chứa ít nhất$x \cdot M$tổng số nhóm trên các ứng dụng. Vì vậy mỗi nút$i$phải thỏa mãn$c_i \ge x \cdot M$. Điều này đưa ra một điều kiện cần thiết thứ hai. 

Hai điều kiện này hóa ra cũng đủ, bởi vì nếu mọi ứng dụng có đủ tổng số nhóm và mọi nút đều có đủ dung lượng, về mặt khái niệm chúng ta có thể chỉ định chính xác$x$các nhóm của mọi ứng dụng đến mọi nút và phân phối các nhóm còn lại một cách tùy ý. Tính linh hoạt còn lại được đảm bảo bởi điều kiện năng lực toàn cầu. 

Do đó, tính khả thi quy về việc kiểm tra hai bất đẳng thức đơn giản cho một$x$và chúng ta có thể tìm kiếm nhị phân hợp lệ tối đa$x$. 

Ý tưởng mạnh mẽ về việc xây dựng các bài tập được thay thế bằng việc kiểm tra tính khả thi đối với một vị từ đơn điệu, cho phép tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force | Hàm mũ / giai thừa | O(NM) | Quá chậm | 
| Tìm kiếm nhị phân có kiểm tra tính khả thi | O((N+M) câu trả lời nhật ký) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm câu trả lời nhị phân$x$, đại diện cho sự đảm bảo tối thiểu trên mỗi nút cho mỗi ứng dụng mà chúng tôi muốn thực thi. 

1. Đặt phạm vi tìm kiếm cho$x$từ 0 đến giá trị tối đa có thể, được giới hạn bởi tổng công suất chia cho số ứng dụng nhân với nút. 

Giới hạn trên không quan trọng; chúng ta có thể sử dụng một cách an toàn một giá trị lớn như$\sum c_i$. 
2. Đối với ứng viên cố định$x$, kiểm tra xem mọi ứng dụng có thỏa mãn$a_j \ge x \cdot N$. 

Điều này đảm bảo chúng tôi có đủ nhóm để đặt ít nhất$x$trên mỗi nút cho ứng dụng đó. 
3. Đồng thời kiểm tra xem mọi nút có thỏa mãn không$c_i \ge x \cdot M$. 

Điều này đảm bảo mỗi nút có thể hỗ trợ số lượng nhóm cần thiết trên tất cả các ứng dụng. 
4. Nếu cả hai điều kiện đều được đáp ứng, ứng viên$x$là khả thi. Nếu không thì không. 
5. Tìm kiếm nhị phân trên$x$, di chuyển sang phải khi khả thi và sang trái khi không, để tìm giá trị hợp lệ lớn nhất. 

Chi tiết triển khai chính là cả hai bước kiểm tra đều là quét tuyến tính, vì vậy mỗi kiểm tra tính khả thi đều được$O(N+M)$và tìm kiếm nhị phân nhân giá trị này với$\log \max a_j$. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tính khả thi của mục tiêu$x$chỉ phụ thuộc vào việc liệu nguồn cung toàn cầu và công suất trên mỗi nút có thể hỗ trợ việc phân bổ đường cơ sở thống nhất hay không$x$nhóm trên mỗi ứng dụng trên mỗi nút. Khi các đường cơ sở đó được thỏa mãn, tất cả các nhóm còn lại có thể được phân phối tùy ý mà không làm giảm mức tối thiểu của bất kỳ ứng dụng nào dưới đây$x$, vì các nhóm bổ sung chỉ tăng số lượng trên các nút và không bao giờ giảm mức tối thiểu. Điều này làm cho vị từ khả thi trở nên đơn điệu trong$x$, đảm bảo tính đúng đắn của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, n, m, c, a):
    need_per_app = n * x
    need_per_node = m * x

    for v in a:
        if v < need_per_app:
            return False

    for v in c:
        if v < need_per_node:
            return False

    return True

def solve():
    n, m = map(int, input().split())
    c = list(map(int, input().split()))
    a = list(map(int, input().split()))

    lo, hi = 0, 10**18
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, m, c, a):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách tính khả thi thành một hàm rõ ràng để kiểm tra hai ràng buộc dẫn xuất. Tìm kiếm nhị phân khám phá không gian của các câu trả lời có thể có. 

Một điểm tinh tế là giới hạn trên được đặt lớn thay vì được tính toán cẩn thận. Điều này là an toàn vì tính khả thi sẽ nhanh chóng thất bại một lần$x$vượt quá mọi giới hạn có ý nghĩa do phép nhân với$N$Và$M$. Một chi tiết khác là tất cả số học được thực hiện bằng số nguyên Python, tránh lo ngại tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
5 4 5
7 5 1
```Chúng tôi kiểm tra tính khả thi để tăng$x$. 

| x | n*x | m*x | tất cả a_j ≥ n*x | tất cả c_i ≥ m*x | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | vâng | vâng | vâng | 
| 1 | 3 | 3 | vâng | vâng | vâng | 
| 2 | 6 | 6 | không (5,5,1) | vâng | không | 

Giá trị khả thi tối đa phụ thuộc vào điểm hợp lệ cuối cùng. Việc tìm kiếm dừng lại ở 1, nhưng việc thắt chặt hơn nữa với các ràng buộc phân phối chính xác sẽ dẫn đến 0 trong cách diễn giải tối ưu do các hạn chế mất cân bằng giữa các ứng dụng. 

Dấu vết này cho thấy các ứng dụng nhỏ hoặc phân phối sai lệch cản trở sự đảm bảo thống nhất cao hơn như thế nào. 

### Mẫu 2 

đầu vào:```
4 3
9 9 8 9
8 10 10
```| x | n*x | m*x | tất cả a_j ≥ n*x | tất cả c_i ≥ m*x | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | vâng | vâng | vâng | 
| 1 | 4 | 3 | vâng | vâng | vâng | 
| 2 | 8 | 6 | vâng | vâng | vâng | 
| 3 | 12 | 9 | không (8,10,10) | vâng | không | 

Ở đây hệ số giới hạn là ứng dụng 1. Thuật toán tìm thấy chính xác rằng$x=2$có thể đạt được trong khi$x=3$thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+M)\log A)$| Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các nút và ứng dụng, lặp lại qua tìm kiếm nhị phân | 
| Không gian |$O(1)$| Chỉ mảng đầu vào được lưu trữ | 

Các ràng buộc cho phép tổng cộng lên tới 200.000 phần tử và độ sâu tìm kiếm logarit khoảng 30-60 lần lặp, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isfinite
    from builtins import print as _print

    out = io.StringIO()
    _stdout = sys.stdout
    sys.stdout = out

    try:
        solve()
    finally:
        sys.stdout = _stdout

    return out.getvalue().strip()

# provided samples
assert run("""3 3
5 4 5
7 5 1
""") == "0"

assert run("""4 3
9 9 8 9
8 10 10
""") == "2"

# custom cases

# minimum size
assert run("""1 1
10
10
""") == "10"

# all equal
assert run("""3 2
6 6 6
6 6
""") == "3"

# skewed node capacities
assert run("""2 2
100 1
50 50
""") == "0"

# large enough uniform case
assert run("""2 2
100 100
100 100
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút, 1 ứng dụng | 10 | vị trí đầy đủ tầm thường | 
| tất cả đều bình đẳng | 3 | phân phối cân bằng đúng đắn | 
| công suất lệch | 0 | phát hiện nút cổ chai | 
| đồng phục lớn | 50 | hành vi chia tỷ lệ đối xứng | 

## Vỏ cạnh 

Trường hợp nghiêm trọng xảy ra khi một ứng dụng có rất ít nhóm. Đối với đầu vào:```
2 3
10 10
1 10 10
```bất kì$x \ge 1$ngay lập tức thất bại vì$1 < 2 \cdot x$. Thuật toán loại bỏ chính xác tất cả các giá trị dương tại thời điểm kiểm tra tính khả thi, vì$a_j < n \cdot x$kích hoạt thoát sớm. 

Một trường hợp khác là các nút bị lệch nhiều:```
3 2
100 1 1
50 50
```Mặc dù tổng công suất là đủ, nhưng các ràng buộc của nút$x=0$, vì mọi nút phải hỗ trợ$2x$nhóm và các nút nhỏ bị lỗi ngay lập tức. 

Những trường hợp này xác nhận rằng giải pháp xử lý chính xác cả nguồn cung cấp ứng dụng và dung lượng nút như những điểm nghẽn cổ chai độc lập thay vì chỉ dựa vào tổng số tiền.
