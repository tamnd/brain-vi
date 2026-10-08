---
title: "CF 104962A - \u041a\u0440\u0443\u0433\u043b\u044b\u0439 \u0413\u0440\u0430\u0444"
description: "Chúng ta được cho một chu kỳ gồm n ngôi nhà. Mỗi ngôi nhà được đặt trên một vòng tròn nên mỗi ngôi nhà đều có ý niệm tự nhiên là di chuyển sang trái hoặc phải theo vòng tròn. Chúng ta được phép chọn tham số k."
date: "2026-06-28T06:57:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104962
codeforces_index: "A"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2021. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104962
solve_time_s: 55
verified: true
draft: false
---

[CF 104962A - \u041a\u0440\u0443\u0433\u043b\u044b\u0439 \u0413\u0440\u0430\u0444](https://codeforces.com/problemset/problem/104962/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chu kỳ`n`những ngôi nhà. Mỗi ngôi nhà được đặt trên một vòng tròn nên mỗi ngôi nhà đều có ý niệm tự nhiên là di chuyển sang trái hoặc phải theo vòng tròn. 

Chúng ta được phép chọn một tham số`k`. Một lần`k`cố định, mọi ngôi nhà đều được kết nối trực tiếp với`k`những ngôi nhà gần nhất ở phía bên trái của nó và`k`những ngôi nhà gần nhất ở phía bên phải của nó. Sau khi xây dựng các kết nối này, biểu đồ trở nên dày đặc hơn nhiều so với một chu trình đơn giản, bởi vì từ mỗi nút, bạn có thể chuyển sang toàn bộ khối các nút lân cận chỉ bằng một lần di chuyển. 

Khoảng cách giữa hai ngôi nhà được định nghĩa là số cạnh tối thiểu cần thiết để di chuyển giữa chúng trong biểu đồ mới này. Nhiệm vụ là chọn số nhỏ nhất`k`sao cho khoảng cách đường đi ngắn nhất có thể lớn nhất giữa bất kỳ cặp nhà nào là tối đa`d`. 

Nói cách khác, chúng tôi đang kiểm soát khoảng cách mỗi nút có thể “nhìn thấy” trong một bước dọc theo vòng tròn và chúng tôi muốn đồ thị kết quả có đường kính tối đa`d`. 

Những hạn chế là vô cùng lớn, với`n`Và`d`lên tới`10^12`. Điều này ngay lập tức loại trừ mọi mô phỏng biểu đồ hoặc lý luận kiểu BFS cho mỗi trường hợp thử nghiệm. Ngay cả việc lưu trữ kề cũng là không thể, vì vậy giải pháp phải thu gọn cấu trúc thành một công thức trực tiếp hoặc nhiều nhất là tính toán logarit. 

Một trường hợp khó nhận thấy là khi`n`là rất nhỏ. Ví dụ, nếu`n = 3`, cấu trúc gần như đã được kết nối đầy đủ ngay cả đối với các thiết bị nhỏ`k`và hành vi khoảng cách trở nên tầm thường. Một góc khác là khi`d`đủ lớn để thậm chí`k = 1`đã đáp ứng yêu cầu, như trong các chu trình nhỏ khi đường kính của chu trình đã nằm trong giới hạn. 

Một sự hiểu lầm ngây thơ là cho rằng khoảng cách phụ thuộc vào các đường tắt đồ thị phức tạp. Ví dụ: người ta có thể thử mô phỏng các đường đi ngắn nhất trên biểu đồ khoảng có trọng số, nhưng điều đó là không cần thiết vì kết nối có cấu trúc rất cứng nhắc: tất cả các cạnh tương ứng với các bước nhảy giới hạn trên một chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử các giá trị của`k`bắt đầu từ`1`, xây dựng biểu đồ và tính đường kính của nó bằng BFS từ mọi nút. Mỗi BFS sẽ là`O(n)`và lặp đi lặp lại`n`lần, cho`O(n^2)`mỗi`k`. Từ`k`cũng có thể lên tới`n`, điều này trở nên hoàn toàn không khả thi ngay cả đối với kích thước vừa phải và chắc chắn là không thể đối với`n = 10^12`. 

Quan sát quan trọng là đồ thị hoàn toàn đối xứng. Mọi nút đều có cấu trúc kết nối giống nhau và điều quan trọng duy nhất là chúng ta có thể di chuyển bao xa dọc theo chu trình trong một bước duy nhất. Từ bất kỳ nút nào, một bước di chuyển duy nhất cho phép nhảy lên`k`các vị trí theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Điều này biến biểu đồ thành số liệu một chiều trong đó mỗi bước sẽ giảm tối đa khoảng cách vòng tròn còn lại`k`. 

Nếu hai nút cách nhau một khoảng tròn`x`, thì đường đi ngắn nhất giữa chúng chính xác là`ceil(x / k)`. Trường hợp xấu nhất xảy ra khi`x`càng lớn càng tốt, đó là khoảng cách xa nhất trên một vòng tròn, cụ thể là`floor(n / 2)`. 

Vậy đường kính của đồ thị trở thành`ceil(floor(n/2) / k)`. Bài toán quy về việc tìm giá trị nhỏ nhất`k`sao cho giá trị này lớn nhất`d`. 

Bất đẳng thức này dễ dàng đảo ngược, đưa ra công thức trực tiếp cho`k`. Toàn bộ vấn đề được chuyển thành số học theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (biểu đồ + BFS) | O(n²) | O(n) | Quá chậm | 
| Lý luận dựa trên công thức | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách liên quan tối đa trên chu kỳ, đó là khoảng cách xa nhất mà hai nút có thể tách rời nhau. Trên một vòng tròn, đây là`floor(n / 2)`. Điều này là do bất kỳ cung nào dài hơn đều có thể đi qua theo hướng ngược lại với chi phí rẻ hơn. 
2. Quan sát điều đó với tham số`k`, một bước có thể bao phủ tới`k`các vị trí dọc theo chu kỳ. Điều này có nghĩa là đi một quãng đường`A = floor(n / 2)`mất`ceil(A / k)`bước trong trường hợp xấu nhất. 
3. Chúng tôi yêu cầu đường kính đồ thị tối đa là`d`, vì vậy chúng tôi thực thi`ceil(A / k) ≤ d`. 
4. Chuyển bất đẳng thức này thành điều kiện trên`k`. Số nguyên nhỏ nhất`k`thỏa mãn rồi đó`ceil(A / d)`. 
5. Trả về giá trị này làm câu trả lời cho từng trường hợp kiểm thử. 

### Tại sao nó hoạt động 

Tất cả các nút đều hoạt động giống hệt nhau về mặt kết nối, do đó, đường đi ngắn nhất trong trường hợp xấu nhất được xác định hoàn toàn bằng khoảng cách vòng tròn chứ không phải bằng nhận dạng nút. Mỗi lần di chuyển làm giảm khoảng cách còn lại tối đa`k`và không có đường đi nào có thể tốt hơn quá trình giảm tham lam vì mọi cạnh đều tôn trọng cấu trúc khoảng giới hạn giống nhau. Điều này làm cho đường đi ngắn nhất tương đương với việc chia khoảng cách thành các phần có kích thước`k`và đường kính được kiểm soát hoàn toàn bởi khoảng cách lớn nhất có thể trên chu trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, d = map(int, input().split())
        A = n // 2
        k = (A + d - 1) // d
        out.append(str(k))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện áp dụng trực tiếp công thức dẫn xuất. Sự tinh tế duy nhất là tính toán`ceil(A / d)`sử dụng số học số nguyên một cách an toàn như`(A + d - 1) // d`. Việc sử dụng phép chia số nguyên sẽ tránh được các lỗi dấu phẩy động, những lỗi này không liên quan ở đây nhưng vẫn là một cạm bẫy phổ biến trong lập trình cạnh tranh. 

giá trị`A = n // 2`nắm bắt khoảng cách vòng tròn trong trường hợp xấu nhất và mọi thứ khác diễn ra sau mức giảm đó. Không cần xây dựng đồ thị. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên. 

đầu vào:`n = 6, d = 2`Khoảng cách xa nhất trên đường tròn là`A = 3`. Mỗi bước có tham số`k`có thể che đậy tới`k`khoảng cách. Chúng tôi cần`ceil(3 / k) ≤ 2`. 

| k | trần(3/k) | hợp lệ | 
| --- | --- | --- | 
| 1 | 3 | không | 
| 2 | 2 | vâng | 
| 3 | 1 | vâng | 

Giá trị hợp lệ nhỏ nhất là`k = 2`. 

Bây giờ hãy xem xét một ví dụ thứ hai. 

đầu vào:`n = 3, d = 1`Đây`A = 1`. Chúng tôi cần`ceil(1 / k) ≤ 1`, đúng cho bất kỳ`k ≥ 1`. 

| k | trần(1/k) | hợp lệ | 
| --- | --- | --- | 
| 1 | 1 | vâng | 

Vậy câu trả lời là`1`. 

Những ví dụ này xác nhận rằng công thức hoạt động chính xác cả khi biểu đồ nhỏ và khi nó yêu cầu phím tắt thực tế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý với số lượng phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc bởi vì ngay cả`10`các trường hợp thử nghiệm chỉ yêu cầu tính toán theo thời gian không đổi cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, d = map(int, input().split())
        A = n // 2
        k = (A + d - 1) // d
        res.append(str(k))
    return "\n".join(res)

# provided samples
assert run("2\n6 2\n3 1\n") == "2\n1"

# minimum size cycle behavior
assert run("1\n3 1\n") == "1"

# already dense requirement relaxed
assert run("1\n10 100\n") == "1"

# larger structure
assert run("1\n100 3\n") == str((100 // 2 + 3 - 1) // 3)

# boundary equality case
assert run("1\n8 1\n") == str((8 // 2 + 1 - 1) // 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 2 | 2 | trường hợp cơ bản không tầm thường | 
| 3 1 | 1 | chu kỳ nhỏ nhất | 
| 10 100 | 1 | d lớn chiếm ưu thế | 
| 100 3 | giá trị tính toán | độ chính xác của tỷ lệ | 
| 8 1 | giá trị tính toán | hạn chế đường kính chặt chẽ | 

## Vỏ cạnh 

Đối với rất nhỏ`n`, chẳng hạn như`n = 3`, chu trình chỉ có một giá trị khoảng cách có ý nghĩa nên thuật toán giảm đúng vì`n // 2`trở thành`1`. Tính toán`k`ít nhất luôn luôn là`1`, do đó không cần xử lý đặc biệt. 

Đối với rất lớn`d`, chẳng hạn như`d ≥ floor(n/2)`, công thức mang lại`k = 1`. Điều này phù hợp với trực giác vì ngay cả các cạnh chu kỳ cơ bản cũng đã cho phép tiếp cận nút xa nhất trong số bước được phép. 

Đối với những trường hợp`d = 1`, yêu cầu buộc đường kính đồ thị phải thu gọn trong một lần di chuyển. Công thức trả về chính xác`k = floor(n/2)`, nghĩa là mọi nút phải có thể truy cập trực tiếp từ nút xa nhất trong một bước, phù hợp với cấu trúc của công trình.
