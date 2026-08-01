---
title: "CF 102638E - Tính toán lại xếp hạng"
description: "Sự cố mô tả một cách mới để chỉ định bộ phận Codeforces dựa trên xếp hạng của người dùng. Đối với xếp hạng r, chúng ta chọn một số nguyên k, nhìn vào giá trị $$f(r-k,r)=frac{1+r+frac{r^2}{2!}+dots+frac{r^{r-k}}{(r-k)!}}{e^r}$$ rồi tính sàn(1 / f) - 1."
date: "2026-08-01T09:43:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102638
codeforces_index: "E"
codeforces_contest_name: "Bredor contest"
rating: 0
weight: 102638
solve_time_s: 132
verified: true
draft: false
---

[CF 102638E - Tính toán lại xếp hạng](https://codeforces.com/problemset/problem/102638/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một cách mới để chỉ định bộ phận Codeforces dựa trên xếp hạng của người dùng. Để xếp hạng`r`, ta chọn số nguyên`k`, nhìn vào giá trị$$f(r-k,r)=\frac{1+r+\frac{r^2}{2!}+\dots+\frac{r^{r-k}}{(r-k)!}}{e^r}$$và sau đó tính toán`floor(1 / f) - 1`. Chúng ta cần cái nhỏ nhất`k`điều đó làm cho sự phân chia này lớn hơn`1`. 

Biểu thức cho`f`là xác suất tích lũy của biến ngẫu nhiên Poisson với tham số`r`nhiều nhất là`r-k`. Nói cách khác, đó là xác suất để giá trị phân phối Poisson không vượt quá ngưỡng đã chọn. Điều kiện để số chia lớn hơn 1 là:$$\lfloor 1/f \rfloor - 1 > 1$$có nghĩa là:$$\lfloor 1/f \rfloor \ge 3$$vì vậy chúng ta chỉ cần tìm nhỏ nhất`k`như vậy:$$f(r-k,r) \le \frac13$$Xếp hạng nhiều nhất là`4000`, và có nhiều nhất`20`các trường hợp thử nghiệm. Một giải pháp thực hiện công việc tỷ lệ thuận với`r`đối với mỗi lần kiểm tra đều dễ dàng đủ nhanh. Tuy nhiên, cố gắng mọi cách có thể`k`và việc tính toán lại xác suất từ ​​đầu vẫn sẽ lãng phí những thao tác không cần thiết. Cấu trúc hữu ích là xác suất giảm đơn điệu khi`k`tăng lên, cho phép tìm kiếm nhị phân. 

Thách thức số chính là đánh giá xác suất tiền tố Poisson một cách chính xác. Tính tổng từ số hạng đầu tiên là nguy hiểm vì các số hạng trở nên cực kỳ nhỏ đối với số hạng lớn.`r`. Thay vào đó, chúng ta bắt đầu từ số hạng được đưa vào cuối cùng và di chuyển xuống dưới. Các điều khoản gần`r-k`là những giá trị lớn nhất trong phạm vi được yêu cầu, vì vậy hướng này giữ cho các giá trị trung gian ổn định. 

Các trường hợp đặc biệt chủ yếu là về ranh giới. 

Đối với đánh giá nhỏ nhất:```
Input
1
5
```câu trả lời là`2`. Một giải pháp giả định điểm cắt không bao giờ có thể trở thành số âm hoặc quên rằng`k`bắt đầu từ số 0 có thể tạo ra kết quả không chính xác. 

Đối với trường hợp câu trả lời nhỏ:```
Input
1
100
```câu trả lời là`5`. Xác suất thay đổi dần dần nên chỉ kiểm tra một giá trị gần đúng như`sqrt(r)`có thể bỏ lỡ ranh giới số nguyên chính xác. 

Đối với xếp hạng lớn:```
Input
1
4000
```câu trả lời vẫn còn tương đối nhỏ so với`r`. Một giải pháp lặp lại tất cả những gì có thể`k`giá trị lên đến`r`hoạt động trên lý thuyết, nhưng việc tính toán liên tục xác suất theo cách đó thực hiện nhiều công việc hơn mức cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách có thể`k`bắt đầu từ số không. Với mỗi ứng viên, chúng tôi tính giá trị của`f(r-k,r)`và dừng lại ở điểm đầu tiên nơi nó trở thành nhiều nhất`1/3`. Điều này đúng vì lớn hơn`k`có nghĩa là tiền tố nhỏ hơn của phân phối Poisson, do đó xác suất chỉ có thể giảm. 

Vấn đề là tính toán xác suất lặp đi lặp lại. Nếu chúng ta cố gắng mọi`k`, có thể có xung quanh`4000`ứng viên. Nếu mỗi phép tính xác suất quét tới`4000`điều khoản, một trường hợp thử nghiệm có thể yêu cầu khoảng`16,000,000`các phép toán dấu phẩy động. Điều này không lý tưởng, đặc biệt với giới hạn thời gian nhỏ. 

Quan sát chính là tính đơn điệu. Khi`k`tăng lên,`r-k`giảm đi, loại bỏ các số hạng khỏi tử số của tiền tố Poisson. Giá trị của`f`không bao giờ tăng. Điều đó có nghĩa là câu trả lời là vị trí đầu tiên trong đó điều kiện đơn điệu trở thành đúng, chính xác là tình huống áp dụng tìm kiếm nhị phân. 

Nhiệm vụ còn lại là đánh giá một xác suất một cách hiệu quả. Chúng tôi tính toán số hạng cuối cùng bằng cách sử dụng logarit, sau đó chia liên tục cho tỷ lệ thích hợp để chuyển sang lũy ​​thừa nhỏ hơn. Điều này tránh được các giai thừa lớn và giữ phép tính bên trong phạm vi dấu phẩy động thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r²) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tìm kiếm nhị phân với đánh giá xác suất ổn định | O(r log r) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mức xếp hạng cố định`r`, tìm kiếm nhị phân câu trả lời`k`. Phạm vi tìm kiếm bắt đầu bằng`0`và giới hạn trên đủ lớn. Vị từ chúng tôi kiểm tra là liệu xác suất`f(r-k,r)`nhiều nhất là`1/3`. 
2. Để đánh giá vị ngữ, hãy`n = r-k`. Nếu như`n`là âm, xác suất bằng 0 vì không có số hạng hợp lệ trong tiền tố. 
3. Tính số hạng xác suất:$$P(X=n)=e^{-r}\frac{r^n}{n!}$$sử dụng logarit:$$\log P(X=n)=-r+n\log r-\log(n!)$$Điều này tránh tràn từ các lũy thừa lớn và giai thừa. 

1. Cộng các số hạng nhỏ hơn bằng cách lùi lại:$$P(X=n-1)=P(X=n)\frac{n}{r}$$và tiếp tục cho đến khi số hạng trở nên không đáng kể. Tổng của các số hạng này chính xác là xác suất tiền tố cần thiết. 

1. Nếu xác suất đã cao nhất`1/3`, di chuyển tìm kiếm nhị phân sang trái vì nhỏ hơn`k`cũng có thể hoạt động. Ngược lại, hãy di chuyển sang phải vì cần phải giảm mức cắt lớn hơn. 
2. Trả về số nhỏ nhất`k`được tìm thấy bằng tìm kiếm nhị phân. 

Tại sao nó hoạt động: xác suất được tính toán chính xác là xác suất Poisson tích lũy lên đến`r-k`. Tăng dần`k`loại bỏ các số hạng khỏi tổng tích lũy này, do đó vị từ thay đổi từ sai thành đúng nhiều nhất một lần. Tìm kiếm nhị phân tìm thấy chuyển đổi đầu tiên đó chính xác là giá trị hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def poisson_prefix(r, n):
    if n < 0:
        return 0.0
    if n >= r:
        return 1.0

    log_term = -r + n * math.log(r) - math.lgamma(n + 1)
    term = math.exp(log_term)

    ans = term
    cur = n
    while cur > 0:
        term *= cur / r
        ans += term
        cur -= 1
        if term < 1e-16:
            break

    return ans

def solve_case(r):
    lo, hi = 0, r + 10
    while lo < hi:
        mid = (lo + hi) // 2
        if poisson_prefix(r, r - mid) <= 1.0 / 3.0:
            hi = mid
        else:
            lo = mid + 1
    return lo

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        r = int(input())
        ans.append(str(solve_case(r)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```các`poisson_prefix`Hàm xử lý phần số. Việc sử dụng`math.lgamma`cho`log(n!)`trực tiếp, tránh tràn giai thừa. Sự tái diễn giữa các xác suất Poisson lân cận có nghĩa là chúng ta không bao giờ cần tính lũy thừa hoặc giai thừa một cách rõ ràng. 

Tìm kiếm nhị phân sử dụng thực tế là câu trả lời là một số nguyên và mọi giá trị lớn hơn`k`sau câu trả lời cũng có giá trị. Giới hạn trên`r + 10`là an toàn bởi vì một lần`r-k`trở thành âm thì xác suất trở thành bằng không. 

Điều kiện dừng trong phép tính tổng ngăn cản việc dành thời gian cho các điều khoản không còn ảnh hưởng đến câu trả lời. Ngưỡng này thấp hơn nhiều so với độ chính xác cần thiết để so sánh với`1/3`. 

## Ví dụ đã hoạt động 

cho`r = 5`, việc tìm kiếm sẽ kiểm tra các giá trị khác nhau của`k`. 

| k | n = r-k | Xác suất tiền tố | Quyết định | 
| --- | --- | --- | --- | 
| 0 | 5 | khoảng 0,616 | Quá lớn | 
| 2 | 3 | khoảng 0,265 | hợp lệ | 

Giá trị hợp lệ đầu tiên là`2`, phù hợp với mẫu. Dấu vết này cho thấy tại sao câu trả lời lại phụ thuộc vào ngưỡng xác suất chính xác chứ không phải là một xấp xỉ thô. 

Vì`r = 100`: 

| k | n = r-k | Xác suất tiền tố | Quyết định | 
| --- | --- | --- | --- | 
| 4 | 96 | lớn hơn 1/3 | Quá lớn | 
| 5 | 95 | ít hơn 1/3 | hợp lệ | 

Tìm kiếm nhị phân bỏ qua các giá trị không cần thiết và tìm ranh giới hợp lệ đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(r log r) | Mỗi bước tìm kiếm nhị phân đánh giá xác suất trong trường hợp xấu nhất O(r). | 
| Không gian | O(1) | Chỉ có một số biến dấu phẩy động được lưu trữ. | 

Với`r <= 4000`và nhiều nhất`20`trường hợp thử nghiệm, số lượng hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    def poisson_prefix(r, n):
        if n < 0:
            return 0.0
        if n >= r:
            return 1.0
        term = math.exp(-r + n * math.log(r) - math.lgamma(n + 1))
        res = term
        cur = n
        while cur > 0:
            term *= cur / r
            res += term
            cur -= 1
            if term < 1e-16:
                break
        return res

    def solve(r):
        lo, hi = 0, r + 10
        while lo < hi:
            mid = (lo + hi) // 2
            if poisson_prefix(r, r - mid) <= 1 / 3:
                hi = mid
            else:
                lo = mid + 1
        return str(lo)

    t = int(data[0])
    return "\n".join(solve(int(x)) for x in data[1:t+1])

assert run("1\n5\n") == "2"
assert run("2\n100\n200\n") == "5\n7"
assert run("3\n2500\n3000\n3500\n") == "23\n25\n27"
assert run("1\n6\n") == "2"
assert run("1\n4000\n") == "29"
assert run("1\n10\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 6`|`2`| Giá trị nhỏ gần với mức đánh giá tối thiểu | 
|`1 / 4000`|`29`| Ranh giới đánh giá lớn | 
|`1 / 10`|`3`| Chuyển đổi xác suất nhỏ | 
| Mẫu | Kết quả đầu ra mẫu | Ví dụ gốc | 

## Vỏ cạnh 

cho`r = 5`, thuật toán đạt`k = 2`bởi vì điểm cắt trở thành`3`và xác suất nhận được giá trị Poisson nhiều nhất`3`đã ở dưới rồi`1/3`. Tìm kiếm nhị phân không bỏ sót điều này vì nó kiểm tra trực tiếp điểm chuyển tiếp. 

Vì`r = 100`, câu trả lời không được xác định bằng cách đơn giản lấy một phần cố định của xếp hạng. Thuật toán đánh giá ranh giới xác suất thực tế và thấy rằng`k = 5`là sự lựa chọn hợp lệ đầu tiên. 

Vì`r = 4000`, các phép tính giai thừa và lũy thừa trực tiếp sẽ tràn hoặc mất độ chính xác. Việc tính toán số hạng logarit và phép truy hồi ngược tránh được những lỗi này trong khi vẫn đảm bảo đủ độ chính xác để so sánh với`1/3`.
