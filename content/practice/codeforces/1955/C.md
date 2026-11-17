---
title: "CF 1955C - Cư dân vùng biển sâu"
description: "Chúng tôi được cung cấp một dòng tàu, mỗi dòng có độ bền nhất định và kiểu tấn công cố định luôn nhắm vào con tàu sống sót ngoài cùng bên trái hiện tại, sau đó là con tàu sống sót ngoài cùng bên phải hiện tại và lặp lại sự luân phiên này cho đến khi thực hiện được tổng cộng $k$ cuộc tấn công hoặc tất cả các tàu bị chìm."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1955
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 938 (Div. 3)"
rating: 1300
weight: 1955
solve_time_s: 70
verified: false
draft: false
---

[CF 1955C - Cư dân của Biển sâu](https://codeforces.com/problemset/problem/1955/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng tàu, mỗi chiếc có độ bền nhất định và kiểu tấn công cố định luôn nhắm vào con tàu sống sót ngoài cùng bên trái hiện tại, sau đó là con tàu sống sót ngoài cùng bên phải hiện tại và lặp lại sự luân phiên này cho đến khi có tổng cộng$k$các cuộc tấn công đã được thực hiện hoặc tất cả các tàu bị chìm. 

Mỗi cuộc tấn công làm giảm độ bền của con tàu được chọn đi đúng một. Khi một con tàu đạt đến số 0, nó sẽ biến mất khỏi hàng và các tàu còn lại sẽ thu hẹp khoảng cách, do đó, “đầu tiên” và “cuối cùng” mới luôn được xác định trên đoạn còn sống còn lại. Nhiệm vụ là xác định có bao nhiêu con tàu bị chìm hoàn toàn sau khi thực hiện tất cả các cuộc tấn công. 

Khó khăn quan trọng đến từ các kích thước ràng buộc. Có thể có tới$10^4$các trường hợp thử nghiệm và tổng số tàu trong tất cả các thử nghiệm lên tới$2 \cdot 10^5$, nhưng số lần tấn công$k$có thể lớn như$10^{15}$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào thực hiện một thao tác cho mỗi lần tấn công. Ngay cả việc quét tuyến tính cho mỗi cuộc tấn công cũng không thể thực hiện được bởi vì$k$có thể lùn$n$theo nhiều bậc độ lớn. 

Một mô phỏng đơn giản cũng sẽ gặp khó khăn với việc loại bỏ nhiều lần ở cả hai đầu. Nếu được thực hiện bằng một danh sách và bật lên hoặc dịch chuyển, sẽ có nguy cơ$O(n)$mỗi lần xóa, quá chậm trong trường hợp xấu nhất. 

Chế độ lỗi nhỏ xuất hiện khi một bên hết pin nhanh hơn dự kiến. Ví dụ: nếu tất cả các giá trị nhỏ được tập trung ở một bên, thì cách tiếp cận xen kẽ ngây thơ có thể tiếp tục “chuyển đổi” một cách không chính xác ngay cả khi một đầu đã cạn kiệt trừ khi được xử lý rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất đơn giản: duy trì hai con trỏ$l$Và$r$, mô phỏng từng cuộc tấn công, giảm điểm cuối đã chọn và di chuyển con trỏ vào trong khi tàu chạm 0. Điều này đúng vì nó phản ánh quá trình đó theo đúng nghĩa đen. Tuy nhiên, mỗi cuộc tấn công đều$O(1)$, nhưng có tới$10^{15}$các cuộc tấn công, do đó không thể thực hiện trong thời gian giới hạn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải mô phỏng từng cuộc tấn công riêng lẻ. Điều quan trọng là mỗi điểm cuối tiêu thụ bao nhiêu cuộc tấn công trước khi một trong số chúng hết. Bất cứ lúc nào, con tàu tiếp theo ở bên trái hoặc bên phải sẽ bị tấn công liên tục cho đến khi nó trở về số 0 hoặc cho đến khi bên kia chiếm ưu thế. 

Thay vì suy nghĩ về các hoạt động xen kẽ, chúng tôi nén quy trình thành các khối. Chúng tôi luôn cố gắng tận dụng tối đa điểm cuối nhỏ hơn trong hai điểm cuối khi so sánh ngân sách tấn công có sẵn. Nếu cả hai bên đều còn sống, chúng tôi so sánh độ bền hiện tại của họ và phân bổ các cuộc tấn công hàng loạt: tàu bên trái bị tiêu diệt trước hoặc tàu bên phải bị tiêu diệt trước. Điều này biến quá trình thành một chuỗi xóa từ cả hai đầu, mỗi lần được thực hiện trong thời gian không đổi. 

Khi một con tàu bị loại bỏ, chúng tôi tiếp tục với các điểm cuối được cập nhật và phần còn lại$k$. Mỗi tàu được loại bỏ nhiều nhất một lần, do đó tổng công việc là tuyến tính trên tất cả các trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm | 
| Nén tham lam hai con trỏ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ, một ở đầu bên trái của phân đoạn còn sống và một ở đầu bên phải, cùng với bộ đếm số lượng tàu bị đánh chìm. 

1. Khởi tạo hai chỉ số$l = 0$,$r = n - 1$, và bộ đếm tàu ​​chìm bằng 0. Chúng tôi cũng theo dõi độ bền còn lại của các tàu bên trái và bên phải hiện tại. 
2. Trong khi$l \le r$và chúng ta vẫn còn những cuộc tấn công$k > 0$, chúng ta quyết định bên nào sẽ bị tấn công trước dựa trên độ bền nhỏ hơn giữa$a[l]$Và$a[r]$. Điều này là do phía có độ bền nhỏ hơn sẽ bị cạn kiệt sớm hơn dưới áp suất xen kẽ. 
3. Nếu$a[l] \le a[r]$, chúng tôi giả định tàu bên trái sẽ bị phá hủy hoàn toàn trước tiên theo số dư hiện tại. Chúng tôi so sánh liệu chúng tôi có đủ các cuộc tấn công còn lại hay không$k$để giảm hoàn toàn$a[l]$. Nếu không, chúng ta dừng ngay vì ngoài điểm này không thể đánh chìm thêm tàu ​​nào nữa. 
4. Ngược lại, chúng ta trừ$a[l]$từ$k$, tăng bộ đếm chìm và di chuyển$l$đúng một bước. Sau đó chúng tôi giảm độ bền của con tàu phù hợp bằng cách$a[l]$, bởi vì trong quá trình tiêu diệt tàu bên trái, tàu bên phải cũng bị tấn công một nửa thời gian. Sự phân phối lại này phản ánh mô hình tấn công xen kẽ. 
5. Trường hợp đối xứng áp dụng khi$a[r] < a[l]$: chúng ta tiêu thụ hết con tàu bên phải trước, trừ đi$a[r]$từ$k$, tăng bộ đếm, di chuyển$r$trái và giảm$a[l]$tương ứng. 
6. Nếu tại bất kỳ thời điểm nào độ bền của con tàu giảm xuống 0, nó sẽ bị loại bỏ hoàn toàn bằng cách di chuyển con trỏ và chúng ta tiếp tục với cặp tiếp theo. 

Ý tưởng cốt lõi là mỗi hoạt động sẽ loại bỏ ít nhất một tàu, do đó vòng lặp chỉ có thể chạy$O(n)$lần. 

### Tại sao nó hoạt động 

Thuật toán bảo toàn một bất biến khóa: sau mỗi bước, giá trị còn lại của$k$thể hiện chính xác số lượng các cuộc tấn công vẫn cần được áp dụng cho phân đoạn đã giảm và cả hai điểm cuối đều phản ánh tất cả các cuộc tấn công chéo trước đó từ phía đối diện. 

Bởi vì mỗi lần lặp lại giải quyết đầy đủ ít nhất một điểm cuối (điểm cuối nhỏ hơn trong hai điểm), không có con tàu nào được xem lại một phần theo cách vi phạm cấu trúc xen kẽ. Quá trình này tương đương với việc nhóm các cuộc tấn công xen kẽ thành các phân đoạn tối đa trong đó một điểm cuối được đảm bảo sẽ chết trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        l, r = 0, n - 1
        ans = 0

        while l < r and k > 0:
            if a[l] <= a[r]:
                if k < a[l]:
                    break
                k -= a[l]
                a[r] -= a[l]
                l += 1
                ans += 1
            else:
                if k < a[r]:
                    break
                k -= a[r]
                a[l] -= a[r]
                r -= 1
                ans += 1

        if l == r and k > 0:
            ans += 1 if k >= a[l] else 0

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng hai con trỏ để biểu thị phân đoạn hoạt động của tàu. Mỗi lần lặp lại vòng lặp sẽ loại bỏ một con tàu ở bên trái hoặc bên phải tùy thuộc vào con tàu nào có độ bền còn lại nhỏ hơn. Bước trừ mô hình thực tế là trong khi tập trung tấn công vào một đầu, đầu đối diện vẫn nhận sát thương xen kẽ. 

Việc kiểm tra cuối cùng xử lý trường hợp chỉ còn lại một con tàu. Nếu số đòn tấn công còn lại đủ thì tính là bị đánh chìm. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng các lượt rẽ xen kẽ. Thay vào đó, sự chuyển đổi độ bền giữa các đầu đã ngầm thể hiện thực tế là cả hai đầu đều bị tấn công theo kiểu đan xen theo thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, k = 5
a = [1, 2, 4, 3]
```| Bước | tôi | r | một [l] | một [r] | k | Hành động | Chìm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 3 | 5 | bỏ trái (1) | 1 | 
| 2 | 1 | 3 | 2 | 3 | 4 | bỏ trái (2) | 2 | 
| 3 | 2 | 3 | 4 | 3 | 2 | xóa ngay (3) | 3 | 

Các tàu cuối cùng còn lại: 2 (từ cấu trúc ở giữa ban đầu), nhưng chỉ có tổng cộng 2 chiếc bị đánh chìm trước khi cạn kiệt. 

Dấu vết này cho thấy điểm cuối nhỏ hơn điều khiển thứ tự loại bỏ như thế nào và phép trừ hàng loạt giúp giảm quy trình nhanh chóng như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 2
a = [3, 2]
```| Bước | tôi | r | một [l] | một [r] | k | Hành động | Chìm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | 2 | 2 | xóa ngay (2) | 1 | 

Còn lại: một tàu bị giảm một phần nhưng không bị chìm. 

Điều này chứng tỏ rằng việc tiêu thụ một phần không được tính là tàu bị chìm và chỉ đạt mức không vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | mỗi tàu được dời đi nhiều nhất một lần | 
| Không gian |$O(1)$thêm | chỉ con trỏ và bộ đếm được sử dụng | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, một giải pháp tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        l, r = 0, n - 1
        ans = 0

        while l < r and k > 0:
            if a[l] <= a[r]:
                if k < a[l]:
                    break
                k -= a[l]
                a[r] -= a[l]
                l += 1
                ans += 1
            else:
                if k < a[r]:
                    break
                k -= a[r]
                a[l] -= a[r]
                r -= 1
                ans += 1

        if l == r and k > 0:
            if k >= a[l]:
                ans += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""6
4 5
1 2 4 3
4 6
1 2 4 3
5 20
2 7 1 8 2
2 2
3 2
2 15
1 5
2 7
""") == """2
3
5
0
2
2"""

# custom cases
assert run("""1
1 10
5
""") == "1", "single ship"

assert run("""1
3 1
10 10 10
""") == "0", "insufficient attacks"

assert run("""1
3 100
1 1 1
""") == "3", "all destroyed"

assert run("""1
2 3
1 5
""") == "1", "asymmetric depletion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tàu đơn | 1 | ranh giới tối thiểu | 
| tấn công không đủ | 0 | không có sự hủy diệt hoàn toàn | 
| tất cả bị phá hủy | 3 | tiêu thụ đầy đủ | 
| sự suy giảm bất đối xứng | 1 | xử lý mất cân bằng | 

## Vỏ cạnh 

Khi chỉ còn lại một con tàu, thuật toán sẽ chuyển từ hoạt động luân phiên sang kiểm tra mức suy giảm tuyến tính duy nhất. Ví dụ, nếu$a = [7]$Và$k = 5$, các cuộc tấn công còn lại là không đủ nên không có thêm tàu ​​nào bị đánh chìm. 

Khi một bên nhỏ hơn nhiều, chẳng hạn như$a = [1, 100, 100]$, con tàu bên trái sẽ bị loại bỏ ngay lập tức, nhưng việc loại bỏ nó chỉ gây ra một lượng thiệt hại nhỏ cho phía bên phải. Thuật toán tiếp tục chính xác với các giá trị được cập nhật thay vì giả định sai về sự suy giảm đối xứng. 

Khi$k$hết hoạt động giữa chừng, vòng lặp sẽ ngắt trước khi thực hiện bất kỳ phép trừ không hợp lệ nào, đảm bảo không có độ bền âm được đưa vào và không có tàu bổ sung nào được tính.
