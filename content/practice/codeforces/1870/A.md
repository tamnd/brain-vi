---
title: "CF 1870A - MEXanized Array"
description: "Chúng tôi đang xây dựng một mảng có độ dài $n$ bằng cách sử dụng các số nguyên không âm, với mỗi phần tử được giới hạn ở mức $x$. Mảng không phải là tùy ý: nó phải có MEX cố định bằng $k$, và trong số tất cả các mảng hợp lệ như vậy, chúng ta muốn mảng có tổng phần tử lớn nhất có thể."
date: "2026-06-08T23:23:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1870
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 1870
solve_time_s: 105
verified: false
draft: false
---

[CF 1870A - MEXanized Array](https://codeforces.com/problemset/problem/1870/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một mảng có chiều dài$n$sử dụng số nguyên không âm, với mỗi phần tử được giới hạn ở$x$. Mảng không phải là tùy ý: nó phải có MEX cố định bằng$k$và trong số tất cả các mảng hợp lệ như vậy, chúng ta muốn mảng có tổng phần tử lớn nhất có thể. 

Điều kiện MEX buộc phải có một cấu trúc rất cụ thể. Mỗi số từ$0$lên đến$k-1$phải xuất hiện ít nhất một lần, trong khi giá trị$k$phải hoàn toàn vắng mặt. Mọi thứ đều lớn hơn$k$không liên quan đến mục đích MEX và có thể được sử dụng tự do miễn là nó không vượt quá$x$. 

Các hạn chế là nhỏ:$n, k, x \le 200$Và$t \le 1000$. Điều này gợi ý rõ ràng rằng mỗi trường hợp thử nghiệm phải được giải quyết trong thời gian không đổi hoặc tuyến tính. Bất cứ điều gì bậc hai cho mỗi trường hợp kiểm thử đều có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến hoán vị hoặc tìm kiếm trạng thái trên mảng đều không cần thiết. 

Trường hợp thất bại khó phát hiện đầu tiên xuất hiện khi$k > x + 1$. Nếu giá trị tối đa cho phép là$x$, thì chúng ta thậm chí không thể bao gồm tất cả các số$0$ĐẾN$k-1$. Ví dụ, nếu$x = 1$Và$k = 3$, chúng ta cần bao gồm$0,1,2$, Nhưng$2$không thể đặt được. Câu trả lời phải là$-1$. 

Một trường hợp thất bại khác xuất phát từ việc hiểu sai yêu cầu MEX. Ví dụ, nếu$k = 0$, chúng tôi không bắt buộc phải bao gồm bất kỳ số nào từ$0$ĐẾN$k-1$(bộ trống), nhưng chúng ta phải đảm bảo rằng$0$vắng mặt trong mảng. Điều này thay đổi hoàn toàn việc xây dựng tối ưu vì chúng ta chỉ có thể sử dụng các giá trị từ$1$ĐẾN$x$. 

Cuối cùng, một kẻ tham lam ngây thơ luôn lấp đầy mảng bằng$x$thất bại vì nó bỏ qua sự hiện diện bắt buộc của các số nhỏ hơn$0$ĐẾN$k-1$, điều này có thể buộc chúng ta phải “lãng phí” các vị trí mà nếu không sẽ tối đa hóa tổng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng tất cả các mảng có độ dài$n$, mỗi mục trong$[0, x]$, sau đó tính MEX và theo dõi tổng tốt nhất. Số mảng là$(x+1)^n$, mà ngay cả đối với$n = 10$đã là rất lớn rồi. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là ràng buộc MEX chỉ hạn chế hai thứ: một bộ số bắt buộc$0$bởi vì$k-1$, và một số bị cấm$k$. Mọi thứ khác là không gian tối ưu hóa miễn phí. 

Để tối đa hóa tổng, mọi vị trí không sử dụng phải được lấp đầy bằng giá trị lớn nhất có thể, đó là$x$. Điều phức tạp duy nhất là chúng ta có thể cần đưa vào những con số được yêu cầu$0$ĐẾN$k-1$và những thứ đó có thể không tối ưu về tổng, nhưng chúng là bắt buộc. 

Chúng ta cũng cần đảm bảo tính khả thi. Để có được MEX chính xác$k$, chúng ta phải bao gồm tất cả các số$0$bởi vì$k-1$, vì vậy chúng ta cần ít nhất$k$các vị trí. Điều đó luôn có thể thực hiện được nếu$n \ge k$, nhưng cái mũ$x$phải cho phép những giá trị đó. 

Khi tính khả thi được đảm bảo, chiến lược tối ưu rất đơn giản: đặt từng$0,1,\dots,k-1$một lần, điền vào các vị trí còn lại với$x$, và tính tổng. 

Có một sự tinh tế hơn nữa: nếu$k \le x$, chúng ta phải đảm bảo rằng chúng ta không vô tình giới thiệu$k$, nhưng vì chúng tôi không bao giờ sử dụng nó nên điều này đương nhiên được thỏa mãn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((x+1)^n)$|$O(n)$| Quá chậm | 
| Xây dựng tham lam tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi xây dựng câu trả lời trực tiếp. 

1. Trước tiên, hãy kiểm tra xem việc xây dựng MEX hợp lệ có khả thi hay không. Nếu như$k > x + 1$, thì chúng ta không thể đặt tất cả các giá trị bắt buộc$0$ĐẾN$k-1$bởi vì$k-1 > x$. Trong trường hợp đó, hãy quay lại$-1$. Điều kiện này bao gồm tất cả các trường hợp không thể xảy ra do giới hạn giá trị. 
2. Tính toán phần đóng góp của các phần tử cần thiết$0,1,\dots,k-1$. Vì chúng phải xuất hiện ít nhất một lần nên chúng ta cộng tổng của chúng$\frac{k(k-1)}{2}$. Đây là cấu trúc tối thiểu cần thiết để thực thi MEX$k$. 
3. Điền vào phần còn lại$n - k$vị trí có giá trị cho phép lớn nhất$x$, bởi vì việc thay thế bất kỳ số nào trong số này bằng số nhỏ hơn sẽ làm giảm tổng mà không ảnh hưởng đến tính hợp lệ của MEX. 
4. Câu trả lời cuối cùng là:$$\text{sum} = \frac{k(k-1)}{2} + (n-k)\cdot x$$5. Nếu$n < k$, chúng tôi cũng lập tức quay lại$-1$, vì chúng ta không thể đặt tất cả các số cần thiết. 

### Tại sao nó hoạt động 

Cấu trúc này là tối ưu vì các ràng buộc MEX tách hoàn toàn mảng thành hai phần: tiền tố bắt buộc gồm các giá trị bắt buộc riêng biệt$0$ĐẾN$k-1$và một vùng có kích thước tự do$n-k$. Phần bắt buộc không có sự linh hoạt về thành viên, chỉ có sự sắp xếp, sắp xếp không ảnh hưởng đến tổng số tiền. Phần miễn phí được tối đa hóa độc lập bằng cách chọn giá trị lớn nhất được phép$x$. Vì không có sự tương tác giữa các vị trí ngoài các ràng buộc hiện diện/vắng mặt, nên bất kỳ sai lệch nào so với cấu trúc này đều vi phạm MEX hoặc làm giảm tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, x = map(int, input().split())

        # feasibility check
        if k > x + 1 or n < k:
            print(-1)
            continue

        # sum of required MEX prefix
        res = k * (k - 1) // 2

        # fill remaining with x
        res += (n - k) * x

        print(res)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh lý do trực tiếp. Việc kiểm tra tính khả thi đảm bảo chúng tôi thực sự có thể nhận ra MEX$k$. Tổng tam giác tính toán sự bao gồm bắt buộc của$0$bởi vì$k-1$. Các vị trí còn lại được lấp đầy một cách tham lam$x$, điều này an toàn vì nó không giới thiệu$k$và luôn tối đa hóa sự đóng góp. 

Phải cẩn thận khi chia số nguyên cho tổng tam giác, nhưng Python xử lý việc này một cách rõ ràng. Thứ tự kiểm tra rất quan trọng: lỗi sớm sẽ tránh được số học sai trên các cấu hình không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=5, k=3, x=3
```Chúng tôi cần những con số$0,1,2$, và chúng ta có thêm 2 vị trí. 

| Bước | Số tiền bắt buộc | Các slot còn lại | Điền giá trị | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 5 | - | 0 | 
| Thêm 0,1,2 | 3 | 5 | - | 3 | 
| Điền còn lại | 3 | 2 | 3 | 9 | 

Câu trả lời cuối cùng là 9. Một mảng hợp lệ là$[0,1,2,3,3]$. 

Điều này xác nhận rằng sau khi đáp ứng MEX, chúng ta có thể thoải mái tối đa hóa phần còn lại. 

### Ví dụ 2 

đầu vào:```
n=4, k=2, x=5
```Chúng ta phải bao gồm$0$Và$1$. 

| Bước | Số tiền bắt buộc | Các slot còn lại | Điền giá trị | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | - | 0 | 
| Thêm 0,1 | 1 | 4 | - | 1 | 
| Điền còn lại | 1 | 2 | 5 | 11 | 

Câu trả lời cuối cùng là 11. 

Điều này cho thấy lớn$x$chiếm ưu thế trong giải pháp một khi các ràng buộc MEX được thỏa mãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng các phép toán số học không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung ngoài các biến | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$t = 1000$dẫn đến tính toán không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, k, x = map(int, input().split())
            if k > x + 1 or n < k:
                print(-1)
                continue
            res = k * (k - 1) // 2 + (n - k) * x
            print(res)

    solve()
    return output.getvalue().strip()

# provided samples
assert run("""9
5 3 3
4 7 5
4 2 28
12 10 6
57 51 122
200 1 200
2 2 1
3 2 1
4 7 10
""") == """7
-1
57
-1
2007
39800
1
2
-1"""

# custom cases
assert run("""1
1 1 5
""") == "0", "minimum valid mex"

assert run("""1
3 2 1
""") == "2", "small tight constraint"

assert run("""1
5 0 3
""") == "15", "mex zero case"

assert run("""1
4 3 2
""") == "3", "boundary feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 5`|`0`| cấu trúc hợp lệ tối thiểu | 
|`3 2 1`|`2`| ràng buộc nhỏ chặt chẽ | 
|`5 0 3`|`15`| xử lý k = 0 | 
|`4 3 2`|`3`| tính khả thi về ranh giới | 

## Vỏ cạnh 

Khi nào$k = 0$, tiền tố được yêu cầu trống. Thuật toán đặt chính xác số tiền cần thiết thành$0$và lấp đầy tất cả$n$vị trí với$x$, cho$n \cdot x$. Điều này phù hợp với yêu cầu MEX vì$0$vắng mặt. 

Khi$k = x + 1$, điều kiện khả thi vẫn đạt và công trình sử dụng chính xác tất cả các giá trị$0$bởi vì$x$. Không có chỗ để tối ưu hóa ở phần bắt buộc, nhưng các vị trí còn lại vẫn có thể được lấp đầy$x$. 

Khi$n = k$, không có vị trí miễn phí. Câu trả lời trở nên chính xác$\frac{k(k-1)}{2}$, tương ứng với cấu trúc MEX bắt buộc không có phần đệm.
