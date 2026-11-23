---
title: "CF 453A - Little Pony và mức tối đa dự kiến"
description: "Chúng ta tung xúc xắc công bằng với các mặt được đánh số từ 1 đến m, đúng n lần. Mỗi cuộn là độc lập và mỗi mặt xuất hiện với xác suất 1/m. Trong số n cuộn đó, chúng ta chỉ nhìn vào giá trị lớn nhất xuất hiện. Nhiệm vụ là tính giá trị kỳ vọng của mức tối đa đó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 453
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 259 (Div. 1)"
rating: 1600
weight: 453
solve_time_s: 106
verified: true
draft: false
---

[CF 453A - Little Pony và mức tối đa dự kiến](https://codeforces.com/problemset/problem/453/A) 

**Đánh giá:** 1600 
**Thẻ:** xác suất 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta tung xúc xắc công bằng với các mặt được đánh số từ`1`ĐẾN`m`, chính xác`n`lần. Mỗi lần cuộn là độc lập và mỗi mặt xuất hiện với xác suất`1 / m`. 

Trong số đó`n`cuộn, chúng tôi chỉ nhìn vào giá trị lớn nhất xuất hiện. Nhiệm vụ là tính giá trị kỳ vọng của mức tối đa đó. 

Đầu vào chứa`m`Và`n`, Ở đâu`m`là số mặt trên xúc xắc và`n`là số lần ném. Đầu ra là một số thực biểu thị kỳ vọng về kết quả tối đa sau tất cả các lần ném. 

Các ràng buộc cho phép cả hai`m`Và`n`lớn như`100000`. Việc liệt kê tất cả các chuỗi cuộn có thể là không thể bởi vì có`m^n`kết quả. Ngay cả đối với các giá trị vừa phải như`m = 100`Và`n = 100`, con số đó lớn về mặt thiên văn. 

Một giải pháp thực hiện công việc tỷ lệ thuận với`m`là hoàn toàn ổn. Một giải pháp tỷ lệ thuận với`m^2`sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa thời hạn. 

Một trường hợp tế nhị xảy ra khi chỉ có một lần ném. 

đầu vào:```
6 1
```Giá trị lớn nhất chỉ đơn giản là kết quả của xúc xắc nên câu trả lời là giá trị trung bình của các số`1..6`, bằng`3.5`. 

Một trường hợp dễ bỏ sót khác là xúc xắc chỉ có một mặt. 

đầu vào:```
1 100000
```Mỗi cuộn luôn`1`, vậy giá trị lớn nhất luôn là`1`và giá trị mong đợi chính xác là`1`. 

Một lỗi phổ biến là cố tính xác suất để giá trị lớn nhất bằng`k`trực tiếp và mắc từng lỗi một. Ví dụ: 

đầu vào:```
2 2
```Tối đa bằng`1`chỉ khi cả hai cuộn đều`1`, điều đó xảy ra với xác suất`1/4`. Tối đa bằng`2`với xác suất`3/4`. Kỳ vọng là$$1 \cdot \frac14 + 2 \cdot \frac34 = 1.75.$$Việc xử lý sai xác suất tích lũy thường tạo ra một kết quả khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi chuỗi cuộn có thể có, tính mức tối đa và tính trung bình tất cả các cực đại. Điều này đúng vì kỳ vọng đơn giản là giá trị trung bình được tính theo xác suất. Thật không may, có`m^n`những kết quả có thể xảy ra. Với`m = n = 100000`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cực đại dễ suy luận hơn nhiều thông qua xác suất tích lũy. 

Giả sử chúng ta muốn xác suất để giá trị lớn nhất lớn nhất là`k`. Mỗi cuộn phải có nhiều nhất`k`. Vì mỗi cuộn độc lập rơi vào`{1,2,...,k}`với xác suất`k/m`, chúng tôi nhận được$$P(\max \le k) = \left(\frac{k}{m}\right)^n.$$Từ đó, chúng ta có thể thu được xác suất để giá trị cực đại chính xác là`k`:$$P(\max = k) = P(\max \le k) - P(\max \le k-1) = \left(\frac{k}{m}\right)^n - \left(\frac{k-1}{m}\right)^n.$$Giá trị mong đợi khi đó là$$E[\max] = \sum_{k=1}^{m} k \cdot P(\max = k).$$Thay thế công thức trên sẽ cho kết quả`O(m)`giải pháp. 

Từ`m ≤ 100000`, đánh giá một lũy thừa và một vài phép tính số học cho mỗi giá trị của`k`dễ dàng phù hợp trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`m`Và`n`. 
2. Khởi tạo`answer = 0`. 
3. Với mọi giá trị`k`từ`1`ĐẾN`m`, tính toán$$p_k = \left(\frac{k}{m}\right)^n$$đó là xác suất để mức tối đa không vượt quá`k`. 
4. Tính toán$$p = p_k - p_{k-1}$$Ở đâu$$p_{k-1}=\left(\frac{k-1}{m}\right)^n.$$Điều này mang lại xác suất để mức tối đa chính xác là`k`. 
5. Thêm$$k \cdot p$$đến câu trả lời đang chạy. 
6. Sau khi xử lý tất cả các giá trị của`k`, in kỳ vọng tích lũy. 

### Tại sao nó hoạt động 

Với mọi số nguyên`k`, sự kiện "`maximum ≤ k`" xảy ra chính xác khi mỗi lượt quay nhiều nhất`k`. Bởi vì các cuộn độc lập nên xác suất của nó là$$\left(\frac{k}{m}\right)^n.$$Các sự kiện"`maximum = k`" phân vùng toàn bộ không gian mẫu. Xác suất của chúng có được bằng cách trừ đi các xác suất tích lũy liên tiếp:$$P(\max = k) = P(\max \le k)-P(\max \le k-1).$$Công thức kỳ vọng$$E[X] = \sum_x x \cdot P(X=x)$$được áp dụng cho biến ngẫu nhiên "cuộn tối đa" sẽ mang lại chính xác giá trị được thuật toán tích lũy. Mỗi mức tối đa có thể đóng góp giá trị của nó nhân với xác suất của nó, do đó tổng cuối cùng bằng mức tối đa dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())

    ans = 0.0

    for k in range(1, m + 1):
        p1 = (k / m) ** n
        p0 = ((k - 1) / m) ** n
        ans += k * (p1 - p0)

    print("{:.12f}".format(ans))

solve()
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Vòng lặp lặp trên tất cả các giá trị có thể có của mức tối đa. Đối với mỗi`k`, nó tính xác suất tích lũy mà tất cả các cuộn nhiều nhất đều`k`, sau đó trừ đi xác suất tương ứng cho`k - 1`. Sự khác biệt chính xác là xác suất mà mức tối đa bằng`k`. 

Sử dụng số học dấu phẩy động là đủ vì dung sai yêu cầu chỉ là`10^{-4}`. Kiểu dấu phẩy động có độ chính xác kép của Python dễ dàng mang lại độ chính xác cao hơn nhiều. 

Biểu thức cho`k = 1`hoạt động tự nhiên bởi vì$$\left(\frac{0}{m}\right)^n = 0.$$Không cần xử lý đặc biệt. 

Việc in mười hai chữ số sau dấu thập phân là quá đủ để làm hài lòng ban giám khảo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 1
```Đối với một cuộn duy nhất, số tiền tối đa chỉ đơn giản là cuộn đó. 

| k |$(k/6)^1$|$((k-1)/6)^1$| Xác suất max = k | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1/6 | 0 | 1/6 | 1/6 | 
| 2 | 6/2 | 1/6 | 1/6 | 6/2 | 
| 3 | 6/3 | 6/2 | 1/6 | 6/3 | 
| 4 | 6/4 | 6/3 | 1/6 | 6/4 | 
| 5 | 6/5 | 6/4 | 1/6 | 6/5 | 
| 6 | 6/6 | 6/5 | 1/6 | 6/6 | 

Tổng hợp những đóng góp mang lại$$\frac{1+2+3+4+5+6}{6} = 3.5.$$Điều này xác nhận rằng công thức giảm xuống mức trung bình thông thường khi chỉ thực hiện một lần ném. 

### Ví dụ 2 

đầu vào:```
2 2
```| k |$(k/2)^2$|$((k-1)/2)^2$| Xác suất max = k | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1/4 | 0 | 1/4 | 0,25 | 
| 2 | 1 | 1/4 | 3/4 | 1,50 | 

Câu trả lời cuối cùng là$$0.25 + 1.50 = 1.75.$$Ví dụ này cho thấy xác suất tích lũy được chuyển đổi thành xác suất chính xác cho mỗi mức tối đa có thể như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| Một lần lặp cho mọi giá trị tối đa có thể có | 
| Không gian |$O(1)$| Chỉ có một số biến dấu phẩy động được lưu trữ | 

Với`m ≤ 100000`, thuật toán chỉ thực hiện một trăm nghìn lần lặp. Điều này dễ dàng đủ nhanh trong giới hạn thời gian và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        m, n = map(int, input().split())

        ans = 0.0
        for k in range(1, m + 1):
            ans += k * ((k / m) ** n - ((k - 1) / m) ** n)

        return "{:.12f}".format(ans)

    return solve()

# provided sample
assert run("6 1\n") == "3.500000000000", "sample"

# minimum size
assert run("1 1\n") == "1.000000000000", "single face, single roll"

# single-face die
assert run("1 100000\n") == "1.000000000000", "maximum always equals 1"

# small hand-checkable case
assert run("2 2\n") == "1.750000000000", "enumerable probability check"

# another boundary-style case
out = float(run("2 1\n"))
assert abs(out - 1.5) < 1e-12, "ordinary average of a two-faced die"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1.000000000000`| Đầu vào nhỏ nhất có thể | 
|`1 100000`|`1.000000000000`| Chết thoái hóa một mặt | 
|`2 2`|`1.750000000000`| Chuyển đổi đúng từ xác suất tích lũy | 
|`2 1`|`1.500000000000`| Hành vi cuộn đơn | 

## Vỏ cạnh 

Xem xét đầu vào```
1 100000
```Vòng lặp chỉ chạy cho`k = 1`.$$(1/1)^{100000}=1, \qquad (0/1)^{100000}=0.$$Xác suất để giá trị lớn nhất bằng`1`là`1`, vậy phần đóng góp là$$1 \cdot 1 = 1.$$Thuật toán xuất ra chính xác`1`. 

Xem xét đầu vào```
6 1
```Đối với mọi`k`, chênh lệch xác suất trở thành$$\frac{k}{6}-\frac{k-1}{6} = \frac16.$$Mỗi mặt đều đóng góp với xác suất bằng nhau, tạo ra mức trung bình tiêu chuẩn của một con súc sắc công bằng. Đầu ra là`3.5`. 

Xem xét đầu vào```
2 2
```Xác suất tích lũy là$$P(\max \le 1)=\left(\frac12\right)^2=\frac14,$$

$$P(\max \le 2)=1.$$Trừ các giá trị liên tiếp mang lại xác suất`1/4`Và`3/4`cho cực đại`1`Và`2`. Tổng hợp các khoản đóng góp có trọng số mang lại`1.75`, khớp chính xác với phân bố thực sự của mức tối đa.
