---
title: "CF 105003B - Vẻ đẹp"
description: "Chúng ta được cung cấp một danh sách các số và một số thao tác hoán đổi cố định. Hoạt động hoán đổi có nghĩa là chọn bất kỳ hai vị trí nào và trao đổi giá trị của chúng."
date: "2026-06-28T03:16:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105003
codeforces_index: "B"
codeforces_contest_name: "XXVIII Spain Olympiad in Informatics, Online Qualifier"
rating: 0
weight: 105003
solve_time_s: 100
verified: false
draft: false
---

[CF 105003B - Vẻ đẹp](https://codeforces.com/problemset/problem/105003/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số và một số thao tác hoán đổi cố định. Hoạt động hoán đổi có nghĩa là chọn bất kỳ hai vị trí nào và trao đổi giá trị của chúng. Sau khi thực hiện chính xác$k$những lần hoán đổi như vậy, chúng tôi đánh giá mảng bằng cách sử dụng điểm xen kẽ: các phần tử ở vị trí 1, 3, 5, … được thêm vào, trong khi các phần tử ở vị trí 2, 4, 6, … bị trừ đi. 

Nhiệm vụ là tối đa hóa số điểm xen kẽ này sau khi thực hiện chính xác$k$trao đổi. 

Điểm mấu chốt là việc hoán đổi không hạn chế giá trị nào có thể kết thúc ở vị trí nào theo bất kỳ cách có ý nghĩa nào khi chúng ta được phép thực hiện nhiều thao tác. Hoán đổi là một hoán vị và các hoán đổi lặp đi lặp lại cho phép chúng ta sắp xếp lại các giá trị một cách rất tự do. Tình huống hạn chế duy nhất có thể xảy ra là khi không được phép thực hiện giao dịch hoán đổi nào cả. 

Từ quan điểm phức tạp,$n$có thể lên đến$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, và$k$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng các giao dịch hoán đổi hoặc tìm kiếm trên các hoán vị. Bất kỳ giải pháp nào cũng phải giảm bớt vấn đề thành việc xây dựng trực tiếp sự sắp xếp cuối cùng tốt nhất có thể. 

Một ý tưởng ngây thơ nhưng mang tính hướng dẫn là mô phỏng các giao dịch hoán đổi và cố gắng cải thiện số tiền xen kẽ một cách tham lam. Điều đó thất bại vì hai lý do. Đầu tiên, số lượng trạng thái có thể có là giai thừa$n$, vì vậy ngay cả việc khám phá một khu phố nhỏ cũng không thể thực hiện được. Thứ hai, cải tiến cục bộ không đảm bảo tính tối ưu toàn cục vì việc di chuyển một giá trị lớn sang vị trí dương có thể yêu cầu chuyển một giá trị lớn khác sang vị trí âm. 

Ý tưởng ngây thơ thứ hai là thử tất cả các hoán vị có thể truy cập được bằng$k$trao đổi. Điều này cũng không thành công vì số lượng hoán vị có thể truy cập tăng cực kỳ nhanh và không gian trạng thái vẫn mang tính giai thừa. 

Vấn đề sâu xa hơn là khi cho phép ít nhất một lần hoán đổi, mảng sẽ có thể được cấu hình lại hoàn toàn trong thực tế, vì vậy câu hỏi chính tập trung vào việc chọn cách sắp xếp tốt nhất chứ không phải mô phỏng các hoạt động. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ quan điểm vũ phu. Nếu chúng ta coi mỗi lần hoán đổi là tạo ra một hoán vị mới thì sau đó$k$hoán đổi chúng ta có thể tưởng tượng việc khám phá tất cả các hoán vị có thể truy cập chính xác$k$chuyển vị. Mỗi lần hoán đổi sẽ phân nhánh không gian trạng thái một cách rất lớn, và thậm chí đối với những$n$, điều này bùng nổ tổ hợp. Điều này làm cho việc mô phỏng trực tiếp là không thể. 

Quan sát quan trọng là việc hoán đổi cho phép chúng ta hoàn toàn tự do hoán vị mảng. Bất kỳ hoán vị nào cũng có thể được xây dựng bằng cách sử dụng một chuỗi các hoán đổi và vì$k$lớn và chúng ta không bị hạn chế bởi việc giảm thiểu các giao dịch hoán đổi, vấn đề không thực sự nằm ở con đường đã đi mà là ở sự sắp xếp cuối cùng. 

Khi chúng tôi chấp nhận rằng mọi thỏa thuận đều có thể đạt được bất cứ khi nào có ít nhất một lần hoán đổi được cho phép, việc tối ưu hóa sẽ trở nên độc lập với quy trình hoán đổi. Chúng ta chỉ cần tối đa hóa tổng xen kẽ bằng cách đặt các giá trị lớn ở vị trí dương và các giá trị nhỏ ở vị trí âm. 

Cách tiếp cận bạo lực hoạt động trên lý thuyết vì nó khám phá tất cả các hoán vị có thể tiếp cận, nhưng nó thất bại vì số lượng hoán vị tăng theo giai thừa. Quan sát cho thấy sự hoán đổi ngụ ý sự tự do sắp xếp lại hoàn toàn làm giảm bài toán thành bài toán gán tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n!)$|$O(n)$| Quá chậm | 
| Sắp xếp và phân công một cách tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách giải pháp thành hai chế độ dựa trên việc có cho phép hoán đổi hay không. 

1. Nếu$k = 0$, không thể sửa đổi được. Câu trả lời chỉ đơn giản là tổng xen kẽ của mảng ban đầu. Không có sự tự do, vì vậy chúng tôi đánh giá trực tiếp cấu hình đã cho. 
2. Nếu$k \ge 1$, chúng tôi giả sử tự do sắp xếp lại hoàn toàn. Chúng ta bỏ qua thứ tự ban đầu và thay vào đó xây dựng một sự sắp xếp tối ưu. 
3. Sắp xếp mảng theo thứ tự giảm dần để gán giá trị lớn nhất trước. Điều này đảm bảo rằng khi chúng ta đặt các giá trị vào những vị trí tích cực, chúng ta sẽ tối đa hóa sự đóng góp của chúng. 
4. Xác định cấu trúc của tổng xen kẽ: vị trí 1, 3, 5,… đóng góp dương, còn vị trí 2, 4, 6,… đóng góp âm. 
5. Gán giá trị lớn nhất cho tất cả các vị trí dương trước tiên. Những vị trí này là những vị trí có giá trị nhất trong biểu thức. 
6. Gán các giá trị còn lại vào vị trí âm. Vì tất cả các giá trị đều không âm trong các ràng buộc của bài toán, nên vị trí này sẽ giảm thiểu thiệt hại do phép trừ. 
7. Tính tổng xen kẽ kết quả trực tiếp từ bài tập này. 

Tại sao nó hoạt động: tổng xen kẽ là tuyến tính trong các giá trị mảng, do đó việc tối đa hóa nó sẽ giảm xuống mức tối đa hóa bài toán gán trọng số trong đó các trọng số được cố định theo vị trí. Vị trí có trọng số dương sẽ nhận được giá trị lớn nhất và vị trí có trọng số âm sẽ nhận được giá trị nhỏ nhất. Sắp xếp đảm bảo sự phân công này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k == 0:
            ans = 0
            for i, x in enumerate(a):
                if i % 2 == 0:
                    ans += x
                else:
                    ans -= x
            print(ans)
            continue

        a.sort(reverse=True)

        pos_count = (n + 1) // 2
        neg_count = n // 2

        ans = 0

        for i in range(pos_count):
            ans += a[i]
        for i in range(pos_count, n):
            ans -= a[i]

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xử lý trường hợp suy biến trong đó không cho phép hoán đổi bằng cách tính trực tiếp tổng xen kẽ của mảng ban đầu. Điều này là cần thiết vì việc sắp xếp lại tối ưu là không thể đạt được khi$k = 0$. 

Khi cho phép hoán đổi, mảng được sắp xếp theo thứ tự giảm dần. Nửa đầu của các giá trị đã sắp xếp được gán cho vị trí dương và các giá trị còn lại được gán cho vị trí âm. Điểm phân chia được xác định bởi số lượng vị trí được lập chỉ mục lẻ tồn tại trong mảng. 

Tổng xen kẽ sau đó được tính toán mà không cần xây dựng lại mảng một cách rõ ràng, điều này tránh được chi phí không cần thiết và giữ cho việc triển khai tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó$n = 5$và mảng là$[1, 5, 2, 4, 3]$, với$k \ge 1$. 

Sau khi sắp xếp, chúng tôi nhận được$[5, 4, 3, 2, 1]$. 

Chúng ta có 3 vị trí tích cực và 2 vị trí tiêu cực. 

| Bước | Khe tích cực | Khe âm | Đóng góp | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | [5, 4, 3] | [2, 1] | 5 + 4 + 3 − 2 − 1 | 

Câu trả lời cuối cùng là$9$. 

Dấu vết này cho thấy các giá trị lớn nhất luôn được đặt ở các vị trí dương, chiếm ưu thế trong cấu trúc xen kẽ. 

Bây giờ hãy xem xét một ví dụ thứ hai trong đó$n = 4$, mảng là$[10, 1, 10, 1]$, Và$k = 0$. 

Chúng tôi không sắp xếp lại bất cứ điều gì. 

| Vị trí | Giá trị | Ký tên | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 10 | + | 10 | 
| 2 | 1 | − | −1 | 
| 3 | 10 | + | 10 | 
| 4 | 1 | − | −1 | 

Kết quả cuối cùng là$18$. 

Ví dụ này nhấn mạnh yêu cầu nghiêm ngặt về việc không sửa đổi mảng khi$k = 0$, mặc dù có sự sắp xếp tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm | 
| Không gian |$O(n)$| Lưu trữ cho mảng | 

Các ràng buộc cho phép tổng cộng$2 \cdot 10^5$các phần tử, do đó việc sắp xếp trên tất cả các trường hợp thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            if k == 0:
                ans = 0
                for i, x in enumerate(a):
                    if i % 2 == 0:
                        ans += x
                    else:
                        ans -= x
                print(ans)
                continue

            a.sort(reverse=True)
            pos = (n + 1) // 2
            ans = sum(a[:pos]) - sum(a[pos:])
            print(ans)

    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample-like cases
assert run("1\n5 1\n1 5 2 4 3\n") == "9"
assert run("1\n4 0\n10 1 10 1\n") == "18"

# edge cases
assert run("1\n2 1\n0 100\n") == "100"
assert run("1\n3 0\n1 2 3\n") == "2"
assert run("1\n6 2\n5 4 3 2 1 0\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$k=0$mảng nhỏ | tổng xen kẽ trực tiếp | quy tắc không sắp xếp lại | 
| trường hợp đã được sắp xếp | tham lam phân công ổn định | tính đúng đắn sau khi sắp xếp | 
| trường hợp hoán đổi hai phần tử | hành vi ranh giới | sắp xếp lại không tầm thường nhỏ nhất | 
| giá trị hỗn hợp | chia đúng | logic phân vùng chẵn/lẻ | 

## Vỏ cạnh 

Khi nào$k = 0$, thuật toán không bao giờ sắp xếp và đánh giá trực tiếp cấu trúc ban đầu. Đối với đầu vào như$[3, 1, 2]$, việc tính toán được cố định như$3 - 1 + 2 = 4$, mặc dù việc sắp xếp lại có thể cải thiện nó. Việc tách nhánh đảm bảo ràng buộc này được tôn trọng chính xác. 

Khi$n$là tối thiểu, chẳng hạn như$n = 2$, việc sắp xếp vẫn hoạt động chính xác, nhưng sự phân chia dương-âm suy biến thành một phần tử mỗi bên. Công thức giảm sạch xuống còn$a_1 - a_2$sau khi đặt hàng. 

Khi tất cả các giá trị đều bằng nhau, việc sắp xếp không có hiệu lực và mọi sự sắp xếp đều mang lại kết quả như nhau. Thuật toán tự nhiên trả về giá trị không đổi chính xác mà không cần xử lý đặc biệt.
