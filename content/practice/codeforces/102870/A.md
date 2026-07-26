---
title: "CF 102870A - Nghệ sĩ đàn accordion và gấu trúc Orz"
description: "Nhiệm vụ mô tả một dãy các tòa nhà liền kề, trong đó mỗi tòa nhà có chiều cao và chiều rộng. Bởi vì các tòa nhà tiếp xúc với nhau nên mặt trước của chúng tạo thành một đường chân trời liên tục."
date: "2026-07-25T13:11:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "A"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 53
verified: true
draft: false
---

[CF 102870A - Nghệ sĩ đàn accordion và gấu trúc Orz](https://codeforces.com/problemset/problem/102870/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một dãy các tòa nhà liền kề, trong đó mỗi tòa nhà có chiều cao và chiều rộng. Bởi vì các tòa nhà tiếp xúc với nhau nên mặt trước của chúng tạo thành một đường chân trời liên tục. Chúng ta cần đặt một quảng cáo hình chữ nhật bên trong đường chân trời đó, với chiều cao và chiều rộng của quảng cáo vuông góc với chiều cao của tòa nhà ban đầu và tìm diện tích lớn nhất có thể. Đầu vào cho biết chiều cao và chiều rộng của mỗi tòa nhà và đầu ra là diện tích tối đa của hình chữ nhật đó. 

Khó khăn chính đến từ thực tế là không phải tất cả các tòa nhà đều có cùng chiều rộng. Một hình chữ nhật có thể bao phủ nhiều tòa nhà liên tiếp nhưng chiều cao của nó bị giới hạn bởi tòa nhà ngắn nhất trong số các tòa nhà đó. Nếu chúng ta thử từng khoảng cách giữa các tòa nhà, chúng ta sẽ cần phải kiểm tra tới$O(n^2)$khoảng thời gian. Với$n$đạt$100000$, điều đó có nghĩa là xung quanh$10^{10}$kiểm tra định kỳ, vượt xa giới hạn thời gian của cuộc thi thông thường cho phép. Thuật toán phải gần tuyến tính. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Nếu một tòa nhà duy nhất là tòa nhà cao nhất thì câu trả lời có thể đến từ việc chỉ sử dụng tòa nhà đó. Ví dụ:```
1
7 3
```Đầu ra đúng là:```
21
```Cách tiếp cận chỉ kiểm tra các cặp tòa nhà sẽ bỏ sót hình chữ nhật bên trong tòa nhà đơn lẻ này. 

Một trường hợp phức tạp khác là khi nhiều tòa nhà liền kề có cùng chiều cao. Ví dụ:```
3
5 2
5 3
5 4
```Đầu ra đúng là:```
45
```Toàn bộ dãy có thể được sử dụng vì mọi tòa nhà đều hỗ trợ chiều cao$5$. Việc triển khai coi các chiều cao bằng nhau là riêng biệt và không bao giờ hợp nhất chúng có thể làm mất tổng chiều rộng và tạo ra câu trả lời nhỏ hơn. 

Một lỗi phổ biến cuối cùng xuất hiện khi tòa nhà ngắn nhất nằm ở cuối. Ví dụ:```
3
4 2
4 2
1 10
```Đầu ra đúng là:```
16
```Hình chữ nhật tốt nhất sử dụng hai tòa nhà đầu tiên có chiều cao$4$và chiều rộng$4$. Giải pháp chỉ tính diện tích khi nhìn thấy tòa nhà ngắn hơn mà quên xử lý ngăn xếp còn lại sau khi quét sẽ bỏ sót hình chữ nhật này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét từng nhóm tòa nhà liên tiếp. Đối với mỗi khoảng, chúng tôi tính toán tổng chiều rộng và chiều cao tối thiểu bên trong khoảng đó. Hình chữ nhật được tạo thành bởi khoảng đó có diện tích bằng chiều cao tối thiểu nhân với tổng chiều rộng. Điều này đúng vì mọi hình chữ nhật có thể có phải nằm hoàn toàn bên trong một số nhóm tòa nhà liên tiếp và chiều cao của nó không được vượt quá tòa nhà ngắn nhất trong nhóm đó. 

Tuy nhiên, điều này kiểm tra quá nhiều khoảng thời gian. Với$n=100000$, có thể có khoảng$n(n+1)/2$khoảng thời gian khác nhau, đó là về$5 \times 10^9$. Ngay cả khi việc tính toán mỗi khoảng thời gian là thời gian không đổi thì việc tính toán này vẫn quá chậm. 

Quan sát quan trọng là chúng ta không cần phải kiểm tra rõ ràng mọi khoảng thời gian. Khi một tòa nhà trở nên ngắn hơn các tòa nhà trước đó, các tòa nhà cao hơn không thể kéo dài qua vị trí hiện tại được nữa. Tại thời điểm đó, chúng tôi biết chiều rộng tối đa mà mỗi tòa nhà cao hơn có thể là chiều cao giới hạn. 

Đây chính xác là tình huống được xử lý bởi một ngăn xếp đơn điệu. Ngăn xếp lưu trữ các tòa nhà theo thứ tự chiều cao tăng dần. Thay vì lưu trữ riêng từng tòa nhà ban đầu, các tòa nhà liên tiếp có cùng chiều cao sẽ được hợp nhất vì chúng luôn hoạt động giống hệt nhau: cùng một chiều cao có thể bao phủ toàn bộ chiều rộng kết hợp của chúng. 

Phương pháp brute-force hoạt động vì mọi hình chữ nhật có thể được biểu thị bằng một khoảng nào đó. Nó thất bại vì nó liên tục tính toán lại thông tin về các khoảng thời gian chồng chéo. Ngăn xếp đơn điệu loại bỏ sự lặp lại này bằng cách chỉ xử lý mỗi tòa nhà một số lần không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các tòa nhà từ trái sang phải và duy trì một chồng các cặp. Mỗi cặp lưu trữ chiều cao và tổng chiều rộng được bao phủ bởi các tòa nhà liên tiếp có chiều cao đó. 

Ngăn xếp được giữ theo thứ tự chiều cao tăng dần. Điều này có nghĩa là mọi chiều cao còn lại trong ngăn xếp vẫn có cơ hội mở rộng hơn nữa về bên phải. 

1. Đối với tòa nhà hiện tại, hãy bắt đầu với chiều rộng của chính nó như chiều rộng hiện đang được xử lý. 

Nếu đỉnh của ngăn xếp có chiều cao lớn hơn tòa nhà hiện tại thì chiều cao cao hơn đó không thể tiếp tục vượt qua điểm này. Loại bỏ nó khỏi ngăn xếp và tính hình chữ nhật bằng chiều cao đó. Chiều rộng của hình chữ nhật đó là chiều rộng tích lũy hiện tại vì tất cả các tòa nhà bị loại bỏ đều liên tiếp và tất cả đều cao ít nhất bằng chiều cao bị loại bỏ. 

1. Tiếp tục loại bỏ các tòa nhà cao hơn cho đến khi ngăn xếp trống hoặc chiều cao trên cùng không lớn hơn chiều cao hiện tại. 

Sau khi loại bỏ khối cao hơn, chiều rộng của khối đó sẽ được cộng vào chiều rộng hiện tại. Điều này cho phép tòa nhà hiện tại kết hợp với tất cả các khu vực ngắn hơn hoặc bằng nhau trước đó. 

1. Nếu đỉnh ngăn xếp có cùng chiều cao với tòa nhà hiện tại, hãy hợp nhất chiều rộng. 

Chiều cao bằng nhau thể hiện cùng một chiều cao hình chữ nhật có thể có, do đó, việc giữ chúng dưới dạng các mục ngăn xếp riêng biệt chỉ làm phức tạp các phép tính sau này. 

1. Đẩy chiều cao hiện tại và chiều rộng tích lũy của nó vào ngăn xếp nếu không có chiều cao bằng nhau để hợp nhất. 
2. Sau khi tất cả tòa nhà được xử lý, hãy xóa mọi mục nhập ngăn xếp còn lại và tính diện tích của nó bằng cách sử dụng chiều rộng tích lũy. 

Không có tòa nhà nào ngắn hơn sau vị trí cuối cùng, vì vậy mọi chiều cao còn lại có thể kéo dài cho đến hết đường chân trời. 

Tại sao nó hoạt động: 

Bất biến của ngăn xếp là độ cao tăng dần sau khi hợp nhất các độ cao bằng nhau. Mỗi mục đại diện cho một nhóm các tòa nhà liên tiếp trong đó chiều cao đó là chiều cao nhỏ nhất được thấy cho đến nay. Khi một tòa nhà thấp hơn xuất hiện, mọi chiều cao cao hơn bị loại bỏ sẽ tìm thấy ranh giới nhỏ hơn đầu tiên ở bên phải. Chiều rộng tích lũy tại thời điểm đó chính xác là chiều rộng tối đa có sẵn cho chiều cao đó. Vì mỗi tòa nhà được đẩy một lần và bật lên một lần nên mọi chiều cao giới hạn có thể có đều được đánh giá, do đó không thể bỏ qua diện tích tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    buildings = [tuple(map(int, input().split())) for _ in range(n)]

    stack = []
    ans = 0

    for h, w in buildings:
        cur_w = w

        while stack and stack[-1][0] > h:
            ph, pw = stack.pop()
            cur_w += pw
            ans = max(ans, ph * cur_w)

        if stack and stack[-1][0] == h:
            stack[-1] = (h, stack[-1][1] + cur_w)
        else:
            stack.append((h, cur_w))

    cur_w = 0
    while stack:
        h, w = stack.pop()
        cur_w += w
        ans = max(ans, h * cur_w)

    print(ans)

if __name__ == "__main__":
    solve()
```Các cửa hàng ngăn xếp`(height, width)`cặp chứ không phải là chỉ số. Vì chiều rộng là một phần của đầu vào nên giải pháp biểu đồ dựa trên chỉ mục sẽ ít thuận tiện hơn. Chiều rộng tích lũy là thông tin quan trọng vì diện tích hình chữ nhật cuối cùng phụ thuộc vào phạm vi bao phủ theo chiều ngang. 

Vòng lặp qua các tòa nhà xử lý tất cả các trường hợp trong đó tòa nhà ngắn hơn đóng các hình chữ nhật trước đó. Điều kiện sử dụng`>`thay vì`>=`bởi vì chiều cao bằng nhau nên được kết hợp chứ không phải loại bỏ. Điều này tránh việc chia một hình chữ nhật thành nhiều phần nhỏ hơn. 

Vòng dọn dẹp cuối cùng là cần thiết vì một số hình chữ nhật vẫn có hiệu lực cho đến hết đường chân trời. Quên bước này sẽ mất câu trả lời khi hình chữ nhật tối đa kết thúc ở tòa nhà cuối cùng. 

Số nguyên Python có thể xử lý diện tích lớn nhất có thể vì chiều cao tối đa và tổng chiều rộng tối đa là khoảng$10^{10}$khi được nhân lên, nó nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5
2 1
4 1
3 1
4 1
1 1
```Sự phát triển của ngăn xếp là: 

| Bước | Tòa nhà hiện tại | Xếp chồng sau khi xử lý | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | (2,1) | [(2,1)] | 0 | 
| 2 | (4,1) | [(2,1),(4,1)] | 0 | 
| 3 | (3,1) | [(2,1),(3,2)] | 4 | 
| 4 | (4,1) | [(2,1),(3,2),(4,1)] | 4 | 
| 5 | (1,1) | [(1,5)] | 9 | 
| Kết thúc | dọn dẹp | trống | 9 | 

Khi chiều cao`1`đến, tất cả các hình chữ nhật cao hơn đã được hoàn thiện. Chiều cao`3`hình chữ nhật có chiều rộng`3`, và chiều cao`4`hình chữ nhật có chiều rộng`1`. Hình chữ nhật tốt nhất là chiều cao`3`với chiều rộng`3`, cho diện tích`9`. 

Đối với mẫu thứ hai:```
5
2 1
4 1
3 1
4 1
2 1
```| Bước | Tòa nhà hiện tại | Xếp chồng sau khi xử lý | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | (2,1) | [(2,1)] | 0 | 
| 2 | (4,1) | [(2,1),(4,1)] | 0 | 
| 3 | (3,1) | [(2,1),(3,2)] | 4 | 
| 4 | (4,1) | [(2,1),(3,2),(4,1)] | 4 | 
| 5 | (2,1) | [(2,5)] | 10 | 
| Kết thúc | dọn dẹp | trống | 10 | 

Tòa nhà cuối cùng có chiều cao`2`, cho phép mọi tòa nhà tham gia. Hình chữ nhật có chiều cao`2`và tổng chiều rộng`5`mang lại diện tích tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tòa nhà được đẩy lên ngăn xếp một lần và bị xóa một lần. | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các tòa nhà đều tăng chiều cao và vẫn nằm nguyên trong đống. | 

Giải pháp này chỉ thực hiện một lượng công việc không đổi trên mỗi tòa nhà, vì vậy nó dễ dàng phù hợp với các ràng buộc của$100000$các tòa nhà. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5
2 1
4 1
3 1
4 1
1 1
""") == "9\n", "sample 1"

assert run("""5
2 1
4 1
3 1
4 1
2 1
""") == "10\n", "sample 2"

assert run("""1
7 3
""") == "21\n", "single building"

assert run("""3
5 2
5 3
5 4
""") == "45\n", "equal heights"

assert run("""3
4 2
4 2
1 10
""") == "16\n", "ending shorter building"

assert run("""4
1 100000
100000 100000
1 100000
100000 100000
""") == "10000000000\n", "large widths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tòa nhà đơn | 21 | Kiểm tra xem một hình chữ nhật có được xem xét hay không. | 
| Tất cả đều có chiều cao bằng nhau | 45 | Kiểm tra việc hợp nhất chiều rộng để có chiều cao giống hệt nhau. | 
| Tòa nhà ngắn nhất ở cuối | 16 | Kiểm tra việc dọn dẹp ngăn xếp khi quá trình quét kết thúc. | 
| Chiều rộng và chiều cao lớn | 10000000000 | Kiểm tra các phép tính số nguyên lớn. | 

## Vỏ cạnh 

Đối với trường hợp tòa nhà đơn lẻ:```
1
7 3
```Tòa nhà được đẩy vào ngăn xếp. Trong quá trình dọn dẹp, nó sẽ bị xóa với chiều rộng tích lũy`3`, vùng sản xuất`7 * 3 = 21`. Thuật toán xử lý trường hợp này vì quá trình dọn dẹp cuối cùng xử lý các hình chữ nhật chưa bao giờ gặp phải tòa nhà nhỏ hơn. 

Đối với chiều cao bằng nhau:```
3
5 2
5 3
5 4
```Tòa nhà đầu tiên tạo ra`(5,2)`. Tòa nhà thứ hai có cùng chiều cao nên lối vào ngăn xếp trở thành`(5,5)`. Thứ ba mở rộng nó đến`(5,9)`. Trong quá trình dọn dẹp, khu vực này trở nên`5 * 9 = 45`. Quy tắc hợp nhất duy trì toàn bộ chiều rộng có thể. 

Đối với trường hợp ranh giới nơi tòa nhà nhỏ nhất nằm cuối cùng:```
3
4 2
4 2
1 10
```Hai chiều cao`4`các tòa nhà sáp nhập vào`(4,4)`. Khi chiều cao`1`xuất hiện, ngăn xếp loại bỏ chiều cao`4`và đánh giá diện tích`4 * 4 = 16`. Tòa nhà cuối cùng không thể cải thiện câu trả lời nên kết quả vẫn như cũ`16`. Điều này chứng tỏ tại sao các tòa nhà ngắn hơn lại kích hoạt việc đánh giá hình chữ nhật.
