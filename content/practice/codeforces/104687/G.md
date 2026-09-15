---
title: "CF 104687G - \u041f\u043e\u043a\u0443\u043f\u043a\u0430"
description: "Chúng ta được đưa cho một hàng bút chì, mỗi chiếc có một mức giá và cuối cùng chúng ta phải mua chính xác $k$ trong số chúng. Quá trình này diễn ra tuần tự: chúng tôi quét từ trái sang phải và quyết định tại mỗi vị trí có nên mua cây bút chì đó hay không. Chi phí mua một cây bút chì không chỉ là giá của nó."
date: "2026-06-29T08:47:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "G"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 77
verified: true
draft: false
---

[CF 104687G - \u041f\u043e\u043a\u0443\u043f\u043a\u0430](https://codeforces.com/problemset/problem/104687/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một hàng bút chì, mỗi chiếc có một mức giá và cuối cùng chúng ta phải mua chính xác.$k$của họ. Quá trình này diễn ra tuần tự: chúng tôi quét từ trái sang phải và quyết định tại mỗi vị trí có nên mua cây bút chì đó hay không. 

Chi phí mua một cây bút chì không chỉ là giá của nó. Nếu đó là cây bút chì đầu tiên bạn mua, bạn chỉ phải trả giá của nó. Nếu không phải là chiếc đầu tiên thì việc mua nó còn buộc bạn phải trả thêm một khoản thuế bằng chỉ số của chiếc bút chì đã mua trước đó. Cây bút chì cuối cùng trong chuỗi phải được mua và nó cũng được tặng kèm trong$k$những món đồ đã chọn. 

Vì vậy, chi phí thực tế không chỉ phụ thuộc vào loại bút chì nào được chọn mà còn phụ thuộc vào thứ tự do vị trí của chúng tạo ra. Nhiệm vụ là giảm thiểu tổng chi phí theo các quy tắc này. 

Các ràng buộc cho phép lên đến$n = 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tất cả các kết hợp của$k$những cây bút chì đã chọn. Thậm chí$O(nk)$lập trình động quá chậm trong trường hợp xấu nhất. Giải pháp phải giảm vấn đề xuống mức gần giống với việc sắp xếp hoặc một lần lựa chọn tham lam. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 1$. Trong trường hợp đó, chiếc bút chì cuối cùng là chiếc duy nhất được chọn và không bao giờ phải trả thuế. Cách giải thích ngây thơ về quy định về thuế vẫn có thể thêm sai điều gì đó dựa trên các chỉ số trước đó, nhưng hành vi đúng là câu trả lời chỉ đơn giản là$a_n$. 

Một trường hợp thất bại khác là khi người ta cho rằng thứ tự lựa chọn không quan trọng bằng việc chỉ chọn$k$giá nhỏ nhất. Thuế phụ thuộc vào các chỉ số, do đó việc bỏ qua các chỉ số hoàn toàn dẫn đến lý luận sai lầm trừ khi nó được tiếp thu đúng mục tiêu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng tất cả các cách lựa chọn$k$bút chì bao gồm cái cuối cùng. Đối với mỗi tập hợp con có kích thước$k-1$từ đầu tiên$n-1$bút chì, chúng ta có thể tính toán chi phí bằng cách xây dựng lại đơn đặt hàng và cộng cả giá cả và thuế. Điều này có tác dụng vì quá trình này hoàn toàn mang tính xác định khi tập hợp con được cố định, nhưng số lượng tập hợp con là$\binom{n-1}{k-1}$, trở nên lớn về mặt thiên văn ngay cả đối với mức độ vừa phải$n$. Cách tiếp cận này phá vỡ ngay lập tức ở quy mô lớn. 

Quan sát quan trọng là một khi chúng ta ấn định loại bút chì nào được mua, thứ tự sẽ bị ép buộc bằng cách tăng chỉ số và cơ cấu thuế sẽ trở nên bổ sung. Nếu chỉ số được chọn là$i_1 < i_2 < \dots < i_{k-1} < n$, thì mỗi cây bút chì được chọn ngoại trừ cây bút chì cuối cùng sẽ đóng góp chỉ số của nó đúng một lần dưới dạng thuế, bởi vì đó là "lần mua trước" đúng một lần trong chuỗi. Điều này loại bỏ tất cả sự tương tác giữa các yếu tố được chọn. 

Khi chi phí phân hủy thành tổng đóng góp độc lập cho mỗi chỉ số đã chọn, vấn đề sẽ trở thành việc lựa chọn$k-1$các mặt hàng có chi phí riêng lẻ tối thiểu, trong đó mỗi mặt hàng có trọng số được sửa đổi bao gồm cả giá và chỉ số đóng góp của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{k} \cdot k)$|$O(k)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát cây bút chì cuối cùng$n$phải luôn được đưa vào, vì vậy chúng tôi sửa nó như một phần của giải pháp. Đóng góp của nó luôn$a_n$và nó không bao giờ đóng góp thuế bổ sung vì không có gì tuân theo nó. 
2. Diễn giải lại lựa chọn còn lại: ta phải chọn chính xác$k-1$chỉ số từ$[1, n-1]$. Những điều này xác định phần còn lại của trình tự mua hàng. 
3. Sắp xếp các chỉ số đã chọn một cách khái niệm khi chúng xuất hiện tự nhiên theo thứ tự tăng dần. Thứ tự này bị ép buộc bởi quá trình từ trái sang phải, do đó không có quyết định hoán vị bổ sung nào tồn tại. 
4. Tính phần đóng góp chi phí của tập hợp đã chọn. Mỗi chỉ số được chọn$i$đóng góp giá của nó$a_i$, và cũng đóng góp chỉ số của nó$i$đúng một lần dưới dạng thuế, vì nó sẽ là cây bút chì được chọn trước đó đúng một lần trong chuỗi. 
5. Xác định trọng lượng quy đổi$w_i = a_i + i$. Tổng chi phí trở thành:$$a_n + \sum_{i \in S} (a_i + i)$$Ở đâu$S$là tập hợp của$k-1$các chỉ số đã chọn. 
6. Vấn đề giảm xuống việc lựa chọn$k-1$giá trị nhỏ nhất của$w_i$giữa$i \in [1, n-1]$. 
7. Đầu ra$a_n$cộng với tổng của những thứ này$k-1$trọng số biến đổi nhỏ nhất. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là thuật ngữ thuế chỉ phụ thuộc vào chỉ mục được chọn trước đó và mỗi chỉ mục được chọn sẽ trở thành phần tử trước đó đúng một lần (ngoại trừ phần tử được chọn cuối cùng trước đó).$n$, vẫn được tính một lần). Điều này làm cho việc đóng góp thuế tuyến tính theo các yếu tố thay vì phụ thuộc vào sự chuyển đổi giữa các trạng thái tùy ý. Sau khi được viết lại, hàm chi phí sẽ trở thành một tổng đơn giản trên các trọng số mục độc lập, do đó, mọi giải pháp tối ưu đều phải chọn trọng số sẵn có nhỏ nhất mà không tính đến sự tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    if k == 1:
        print(a[-1])
        return
    
    w = []
    for i in range(n - 1):
        w.append(a[i] + (i + 1))
    
    w.sort()
    
    ans = a[-1] + sum(w[:k - 1])
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức được chuyển đổi. Trường hợp đặc biệt duy nhất là$k = 1$, nơi chúng ta tránh xây dựng hoặc sắp xếp bất cứ thứ gì và ngay lập tức quay trở lại$a_n$. 

Sự dịch chuyển chỉ số$i + 1$rất quan trọng vì các chỉ số bút chì trong bài toán dựa trên 1, trong khi mảng Python dựa trên 0. Việc thiếu sự thay đổi này là nguyên nhân phổ biến gây ra các lỗi ngẫu nhiên làm sai lệch tính toán thuế một cách âm thầm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi tính toán trọng số chuyển đổi: 

| tôi | một [tôi] | w[i] = a[i] + tôi | 
| --- | --- | --- | 
| 1 | 5 | 6 | 
| 2 | 3 | 5 | 
| 3 | 2 | 5 | 
| 4 | 4 | 8 | 

Chúng tôi cần$k-1 = 2$giá trị nhỏ nhất là 5 và 5. 

Tổng số câu trả lời =$a_5 + 5 + 5 = 6 + 10 = 16$. 

Điều này phù hợp với lựa chọn tối ưu trong đó chúng tôi chọn hai đóng góp hiệu quả rẻ nhất trong số bốn cây bút chì đầu tiên. 

### Ví dụ 2 

đầu vào:```

```Chúng ta chỉ phải chọn cây bút chì cuối cùng. 

| tôi | một [tôi] | w[i] | 
| --- | --- | --- | 
| 1 | 10 | 11 | 
| 2 | 1 | 3 | 
| 3 | 100 | 103 | 

Từ$k = 1$, chúng tôi bỏ qua tất cả những người khác và quay trở lại$a_4 = 5$. 

Điều này xác nhận rằng không có thuế được áp dụng khi không có lựa chọn nào trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp trọng số chuyển đổi chiếm ưu thế | 
| Không gian |$O(n)$| Lưu trữ mảng trọng lượng | 

Giải pháp thoải mái phù hợp trong giới hạn cho$n = 10^5$, vì việc sắp xếp và quét tuyến tính đơn lẻ đều nằm trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    if k == 1:
        print(a[-1])
        return
    w = [a[i] + (i + 1) for i in range(n - 1)]
    w.sort()
    print(a[-1] + sum(w[:k - 1]))

# provided sample
assert run("5 3\n5 3 2 4 6\n") == "16"

# minimum n
assert run("1 1\n7\n") == "7"

# k = 1 case
assert run("4 1\n10 1 100 5\n") == "5"

# all equal values
assert run("5 2\n3 3 3 3 3\n") == str(3 + min(1+3, 2+3, 3+3, 4+3))

# increasing costs
assert run("5 2\n1 2 3 4 5\n") == str(5 + min(1+1, 2+2, 3+3, 4+4))

# large structured case
assert run("6 3\n5 4 3 2 1
```
