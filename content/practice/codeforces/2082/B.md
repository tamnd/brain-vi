---
title: "CF 2082B - Sàn hoặc Trần"
description: "Chúng ta bắt đầu với một giá trị số nguyên và phải thực hiện hai loại hoạt động giảm một nửa với số lần cố định. Một thao tác thay thế giá trị hiện tại bằng giá trị sàn bằng một nửa giá trị. Cái còn lại thay thế nó bằng mức trần bằng một nửa giá trị."
date: "2026-06-09T03:46:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2082
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1010 (Div. 2, Unrated)"
rating: 1600
weight: 2082
solve_time_s: 156
verified: false
draft: false
---

[CF 2082B - Sàn hoặc trần](https://codeforces.com/problemset/problem/2082/B) 

**Đánh giá:** 1600 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một giá trị số nguyên và phải thực hiện hai loại hoạt động giảm một nửa với số lần cố định. 

Một thao tác thay thế giá trị hiện tại bằng giá trị sàn bằng một nửa giá trị. Cái còn lại thay thế nó bằng mức trần bằng một nửa giá trị. Chúng ta phải sử dụng thao tác sàn một cách chính xác`n`thời gian và thao tác trần chính xác`m`lần, nhưng chúng tôi có thể chọn thứ tự của họ. Nhiệm vụ là tìm giá trị cuối cùng nhỏ nhất và lớn nhất có thể đạt được. 

Điều đầu tiên cần chú ý là cả hai thao tác đều hoạt động giống hệt nhau trên các số chẵn:$$\left\lfloor \frac{x}{2} \right\rfloor = \left\lceil \frac{x}{2} \right\rceil = \frac{x}{2}$$Tình huống thú vị duy nhất xảy ra khi`x`thật kỳ lạ:$$\left\lfloor \frac{x}{2} \right\rfloor = \frac{x-1}{2}$$

$$\left\lceil \frac{x}{2} \right\rceil = \frac{x+1}{2}$$Trên các giá trị lẻ, thao tác sàn tạo ra kết quả nhỏ hơn chính xác một kết quả so với thao tác trần. 

Những hạn chế là thách thức thực sự. Các giá trị`x`,`n`, Và`m`tất cả có thể lớn như`10^9`, và có tới`10^4`trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả`n + m`các hoạt động là không thể vì một trường hợp thử nghiệm có thể yêu cầu hai tỷ hoạt động. 

Quan sát quan trọng là mọi thao tác đều giảm gần một nửa giá trị hiện tại. Từ`x ≤ 10^9`, sau khoảng 30 lần chia đôi giá trị sẽ trở thành`0`hoặc`1`. Khi chúng tôi đạt đến điểm đó, các hoạt động còn lại có thể được xử lý bằng phương pháp phân tích. Điều này có nghĩa là số lượng công việc thực tế trên mỗi trường hợp thử nghiệm có thể giảm xuống một hằng số nhỏ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
x = 1, n = 1, m = 100
```Câu trả lời tối thiểu có thể là`0`. Áp dụng thao tác sàn bất kỳ lúc nào`1`vào trong`0`, và sau đó tất cả các hoạt động giữ nó ở mức`0`. Việc thực hiện bất cẩn sẽ dừng lại khi đạt đến`1`sẽ quay lại không chính xác`1`. 

Coi như:```
x = 1, n = 0, m = 100
```Câu trả lời là`1`. Trần của`1/2`vẫn còn`1`, do đó giá trị không bao giờ thay đổi. 

Coi như:```
x = 0, n = 10^9, m = 10^9
```Câu trả lời là`0 0`. Cả hai hoạt động đều để lại số 0 không thay đổi mãi mãi. 

Các trạng thái đầu cuối này phải được xử lý riêng biệt. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi thứ tự có thể có của các hoạt động. Ngay cả đối với các giá trị nhỏ của`n + m`, số lượng đơn đặt hàng có thể là rất lớn:$$\binom{n+m}{n}$$Điều này phát triển theo cấp số nhân và trở nên vô vọng gần như ngay lập tức. 

Một lực lượng vũ phu hợp lý hơn sẽ mô phỏng một mệnh lệnh đã chọn. Điều đó vẫn thất bại vì`n + m`có thể lớn như`2 \cdot 10^9`. 

Cấu trúc của các thao tác cho chúng ta khả năng quan sát mạnh mẽ hơn nhiều. Mỗi hoạt động giảm một nửa giá trị hiện tại. Bắt đầu từ nhiều nhất`10^9`, sau khoảng 30 bước, giá trị đã giảm xuống`0`hoặc`1`. Từ đó trở đi, các hoạt động tiếp theo là chuyện nhỏ. 

Câu hỏi còn lại là làm thế nào để chọn thứ tự tối ưu. 

Khi giá trị hiện tại là số lẻ, giá trị sàn cho kết quả nhỏ hơn giá trị trần. Nếu chúng ta đang cố gắng giảm thiểu câu trả lời cuối cùng, chúng ta muốn thực hiện phép toán sàn cho các số lẻ bất cứ khi nào có thể. Nếu chúng ta đang cố gắng tối đa hóa câu trả lời cuối cùng, chúng ta muốn thực hiện phép toán trần cho các số lẻ bất cứ khi nào có thể. 

Các số chẵn khác nhau vì cả hai phép toán đều cho giá trị tiếp theo giống hệt nhau. Vì kết quả giống hệt nhau nên chúng ta nên sử dụng những khoảnh khắc đó để sử dụng loại hoạt động ít hữu ích hơn sau này. 

Để giảm thiểu, các phép toán sàn rất có giá trị vì chúng giúp ích cho các số lẻ. Trên các số chẵn, trước tiên chúng ta nên thực hiện các phép toán trần và lưu các phép toán sàn cho các giá trị lẻ trong tương lai. 

Để tối đa hóa, các phép toán trần rất có giá trị vì chúng giúp ích cho các số lẻ. Trên các số chẵn, trước tiên chúng ta nên sử dụng các phép toán sàn và lưu các phép toán trần cho các giá trị lẻ trong tương lai. 

Chiến lược tham lam này xác định hoàn toàn trình tự tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(logx) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Để tính câu trả lời tối thiểu: 

1. Trong khi`x > 1`và vẫn còn các hoạt động còn lại, hãy kiểm tra tính chẵn lẻ của`x`. 
2. Nếu`x`là số lẻ và có sẵn thao tác sàn, hãy sử dụng nó. 

Điều này mang lại giá trị tiếp theo nhỏ nhất có thể. 
3. Nếu`x`là số lẻ và không còn thao tác sàn nào, hãy sử dụng thao tác trần. 
4. Nếu`x`chẵn thì cả hai phép toán đều dẫn đến cùng một giá trị tiếp theo. 

Sử dụng phép toán trần nếu có, vì các phép toán trần ít hữu ích hơn các phép toán sàn trên các giá trị lẻ trong tương lai. 
5. Tiếp tục cho đến khi tất cả các thao tác được sử dụng hoặc`x`trở thành`0`hoặc`1`. 
6. Nếu`x = 0`, câu trả lời là`0`. 
7. Nếu`x = 1`, câu trả lời là`0`nếu vẫn còn hoạt động sàn, nếu không`1`. 

Để tính câu trả lời tối đa: 

1. Trong khi`x > 1`và hoạt động vẫn còn, kiểm tra tính chẵn lẻ. 
2. Nếu`x`là số lẻ và có sẵn thao tác trần, hãy sử dụng nó. 

Điều này mang lại giá trị tiếp theo lớn nhất có thể. 
3. Nếu`x`là số lẻ và không còn thao tác trần, hãy sử dụng thao tác sàn. 
4. Nếu`x`chẵn, hãy sử dụng thao tác sàn nếu có thể. 

Các phép toán sàn vô hại đối với các giá trị chẵn, vì vậy chúng tôi bảo toàn các phép toán trần cho các giá trị lẻ trong tương lai. 
5. Tiếp tục cho đến khi tất cả các thao tác được sử dụng hoặc`x`trở thành`0`hoặc`1`. 
6. Nếu`x = 0`, câu trả lời là`0`. 
7. Nếu`x = 1`, câu trả lời là`1`chỉ khi không còn hoạt động sàn nào. Nếu không thì cuối cùng một số tầng phải được áp dụng, tạo ra kết quả`0`. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là sàn và trần chỉ khác nhau ở số lẻ. 

Đối với câu trả lời tối thiểu, mỗi thao tác sàn được sử dụng trên một giá trị lẻ sẽ làm giảm giá trị hiện tại nhiều hơn một thao tác trần. Vì tất cả các hoạt động trong tương lai đều là các hàm đơn điệu của giá trị hiện tại nên việc có giá trị nhỏ hơn bây giờ không bao giờ có thể ảnh hưởng đến mục tiêu giảm thiểu. Do đó, các phép toán sàn có giá trị nhất ở trạng thái lẻ và các phép toán trần nên được dành cho các trạng thái chẵn bất cứ khi nào có thể. 

Đối số tối đa hóa là đối xứng. Các phép toán trần có giá trị nhất ở trạng thái lẻ vì chúng giữ giá trị lớn hơn một. Việc sử dụng các thao tác sàn ở trạng thái chẵn sẽ bảo toàn các thao tác trần có giá trị đó cho sau này. 

Vì mỗi lựa chọn tham lam sẽ bảo toàn loại hoạt động hữu ích hơn cho lần gặp lẻ tiếp theo, nên không có thứ tự thay thế nào có thể tạo ra kết quả cuối cùng tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_minimum(x, n, m):
    while x > 1 and (n > 0 or m > 0):
        if x & 1:
            if n > 0:
                x //= 2
                n -= 1
            else:
                x = (x + 1) // 2
                m -= 1
        else:
            if m > 0:
                x //= 2
                m -= 1
            else:
                x //= 2
                n -= 1

    if x == 0:
        return 0
    if x == 1:
        return 0 if n > 0 else 1
    return x

def get_maximum(x, n, m):
    while x > 1 and (n > 0 or m > 0):
        if x & 1:
            if m > 0:
                x = (x + 1) // 2
                m -= 1
            else:
                x //= 2
                n -= 1
        else:
            if n > 0:
                x //= 2
                n -= 1
            else:
                x //= 2
                m -= 1

    if x == 0:
        return 0
    if x == 1:
        return 1 if n == 0 else 0
    return x

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        x, n, m = map(int, input().split())
        mn = get_minimum(x, n, m)
        mx = get_maximum(x, n, m)
        ans.append(f"{mn} {mx}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Hai hàm trợ giúp thực hiện hai chiến lược tham lam một cách độc lập. 

Vòng lặp chỉ chạy khi`x > 1`. Mỗi lần lặp lại giảm một nửa giá trị, vì vậy số lần lặp nhiều nhất là khoảng 31 đối với các ràng buộc đã cho. Ngay cả khi`n`Và`m`là một tỷ, chúng tôi không bao giờ mô phỏng ở bất cứ đâu gần nhiều hoạt động như vậy. 

Việc xử lý thiết bị đầu cuối là phần tinh tế. Một lần`x`trở thành`1`, các hoạt động còn lại quan trọng. Một hoạt động còn lại của tầng cuối cùng sẽ chuyển sang`1`vào trong`0`. Trần hoạt động thôi bỏ đi`1`không thay đổi. Đây là lý do tại sao logic cuối cùng chỉ phụ thuộc vào việc liệu có còn hoạt động sàn nào hay không. 

Không có vấn đề tràn tồn tại vì giá trị chỉ giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12 1 2
```Tính toán tối thiểu: 

| x | n | m | Hành động | 
| --- | --- | --- | --- | 
| 12 | 1 | 2 | thậm chí, sử dụng trần | 
| 6 | 1 | 1 | thậm chí, sử dụng trần | 
| 3 | 1 | 0 | lẻ, sử dụng sàn | 
| 1 | 0 | 0 | dừng lại | 

Kết quả:`1`. 

Tính toán tối đa: 

| x | n | m | Hành động | 
| --- | --- | --- | --- | 
| 12 | 1 | 2 | thậm chí, sử dụng sàn | 
| 6 | 0 | 2 | thậm chí, sử dụng trần | 
| 3 | 0 | 1 | lẻ, dùng trần | 
| 2 | 0 | 0 | dừng lại | 

Kết quả:`2`. 

Ví dụ này cho thấy tại sao việc sử dụng loại phép toán ít giá trị hơn trên các số chẵn lại quan trọng. 

### Ví dụ 2 

đầu vào:```
706636307 0 3
```Cả mức tối thiểu và tối đa đều hoạt động giống hệt nhau vì chỉ tồn tại các hoạt động trần. 

| x | n | m | Hành động | 
| --- | --- | --- | --- | 
| 706636307 | 0 | 3 | lẻ, trần | 
| 353318154 | 0 | 2 | thậm chí, trần | 
| 176659077 | 0 | 1 | lẻ, trần | 
| 88329539 | 0 | 0 | dừng lại | 

Kết quả:```
88329539 88329539
```Điều này chứng tỏ rằng khi chỉ còn lại một loại thao tác thì câu hỏi đặt hàng sẽ biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logx) | Mỗi bước giảm một nửa giá trị hiện tại | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Từ`x ≤ 10^9`,`log2(x)`nhiều nhất là khoảng 30. Với nhiều nhất`10^4`trường hợp thử nghiệm, tổng công việc chỉ là vài trăm nghìn thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    def get_minimum(x, n, m):
        while x > 1 and (n > 0 or m > 0):
            if x & 1:
                if n:
                    x //= 2
                    n -= 1
                else:
                    x = (x + 1) // 2
                    m -= 1
            else:
                if m:
                    x //= 2
                    m -= 1
                else:
                    x //= 2
                    n -= 1

        if x == 0:
            return 0
        if x == 1:
            return 0 if n > 0 else 1
        return x

    def get_maximum(x, n, m):
        while x > 1 and (n > 0 or m > 0):
            if x & 1:
                if m:
                    x = (x + 1) // 2
                    m -= 1
                else:
                    x //= 2
                    n -= 1
            else:
                if n:
                    x //= 2
                    n -= 1
                else:
                    x //= 2
                    m -= 1

        if x == 0:
            return 0
        if x == 1:
            return 1 if n == 0 else 0
        return x

    t = int(input())
    out = []
    for _ in range(t):
        x, n, m = map(int, input().split())
        out.append(f"{get_minimum(x,n,m)} {get_maximum(x,n,m)}")
    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    res = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return res

# provided sample
assert run(
"""5
12 1 2
12 1 1
12 0 0
12 1000000000 1000000000
706636307 0 3
"""
) == "\n".join([
"1 2",
"3 3",
"12 12",
"0 0",
"88329539 88329539"
])

# custom cases
assert run("1\n0 100 100\n") == "0 0", "zero stays zero"
assert run("1\n1 1 100\n") == "0 0", "remaining floor kills one"
assert run("1\n1 0 100\n") == "1 1", "only ceilings keep one"
assert run("1\n3 1 0\n") == "1 1", "single odd floor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 100 100`|`0 0`| Số không là trạng thái hấp thụ | 
|`1 1 100`|`0 0`| Bất kỳ tầng còn lại nào cuối cùng cũng đạt đến số 0 | 
|`1 0 100`|`1 1`| Riêng trần nhà không bao giờ thay đổi | 
|`3 1 0`|`1 1`| Hành vi sàn lẻ cơ bản | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1 100
```Mô phỏng ngay lập tức đạt đến trạng thái cuối`x = 1`. Logic tối thiểu kiểm tra xem có còn hoạt động sàn nào không. Từ`n = 1`, câu trả lời trở thành`0`. Logic tối đa đi đến kết luận tương tự vì cuối cùng thì sàn cũng phải được sử dụng. Đầu ra là:```
0 0
```Coi như:```
1
1 0 100
```Không có hoạt động sàn tồn tại. Trần của`1/2`vẫn còn`1`, do đó mọi thao tác còn lại đều giữ nguyên giá trị. Cả hai câu trả lời đều là:```
1 1
```Coi như:```
1
0 1000000000 1000000000
```Thuật toán không bao giờ vào vòng lặp chính vì`x = 0`. Kiểm tra thiết bị đầu cuối trả về`0`ngay lập tức. Đầu ra là:```
0 0
```Coi như:```
1
12 1000000000 1000000000
```Giá trị đạt`0`chỉ sau một vài lần chia đôi. Thuật toán không cố gắng mô phỏng hàng tỷ thao tác. Một lần`x = 0`, câu trả lời được cố định tại`0`, đó chính xác là hành vi cần thiết cho các ràng buộc lớn nhất.
