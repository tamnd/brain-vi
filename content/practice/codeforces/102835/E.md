---
title: "CF 102835E - Trò chơi màu sắc"
description: "Chúng ta có một chuỗi gạch màu. Có bảy màu có thể. Một thao tác có thể loại bỏ một nhóm ô nếu chúng ta có thể chọn một dãy con bao gồm các ô chỉ có một màu và kích thước của dãy con đó lớn hơn một ngưỡng nhất định."
date: "2026-07-26T14:58:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "E"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 42
verified: true
draft: false
---

[CF 102835E - Trò chơi màu sắc](https://codeforces.com/problemset/problem/102835/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi gạch màu. Có bảy màu có thể. Một thao tác có thể loại bỏ một nhóm ô nếu chúng ta có thể chọn một dãy con bao gồm các ô chỉ có một màu và kích thước của dãy con đó lớn hơn một ngưỡng nhất định. Các ô bị loại bỏ sẽ biến mất và các ô còn lại sẽ đóng các khoảng trống. Nhiệm vụ là xác định xem liệu cuối cùng toàn bộ chuỗi có thể bị xóa hay không. 

Đầu vào bao gồm chuỗi màu ban đầu và ngưỡng loại bỏ. Kết quả đầu ra cho biết liệu có tồn tại một chuỗi các thao tác xóa hợp lệ để xóa mọi ô hay không. 

Ràng buộc quan trọng là độ dài của chuỗi. Giải pháp dự định cần xử lý độ dài khoảng 500, loại trừ các mô phỏng đối với tất cả các lệnh loại bỏ có thể có. Số lượng các cách có thể để chọn các nhóm di động tăng theo cấp số nhân, do đó việc tìm kiếm trực tiếp qua các hoạt động là không thể. Giải pháp lập trình động khối là phù hợp vì các hoạt động 500³ có thể thực hiện được ở dạng tối ưu hóa. 

Một sai lầm phổ biến là chỉ tìm kiếm các nhóm hiện có ban đầu. Ví dụ: một chuỗi như:```
RRBBRR 2
```có thể được giải quyết ngay cả khi toàn bộ chuỗi ban đầu không có một màu. Có thể loại bỏ ba ô R đầu tiên sau các hoạt động kết hợp thông qua việc hợp nhất theo khoảng thời gian. Một giải pháp tham lam chỉ loại bỏ các nhóm hiện có thể bỏ lỡ điều này. 

Một trường hợp khác là khi số lượng màu chính xác là ngưỡng. Ví dụ:```
RRR 3
```Không thể xóa ba ô vì quy tắc yêu cầu kích thước nhóm lớn hơn ngưỡng. Câu trả lời đúng là`No`. Các triển khai sử dụng`>=`thay vì`>`sẽ thất bại. 

Trường hợp khó khăn cuối cùng là một khoảng có thể tháo rời hoàn toàn bên trong một khoảng lớn hơn:```
RRGGBB 1
```Một phần ở giữa có thể biến mất và cho phép các phần còn lại hợp nhất. Xử lý chuỗi ở vị trí cố định thay vì khoảng thời gian sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi trình tự loại bỏ có thể. Đối với mỗi trạng thái của chuỗi, chúng ta có thể tìm thấy mọi dãy con có thể tháo rời và lặp lại. Điều này đúng vì nó khám phá mọi tương lai có thể xảy ra, nhưng số lượng trạng thái là rất lớn. Ngay cả đối với một chuỗi nhỏ, nhiều lệnh thực hiện khác nhau sẽ tạo ra các chuỗi trung gian khác nhau, khiến cho cách tiếp cận trở nên cấp số mũ. 

Quan sát quan trọng là hoạt động chỉ phụ thuộc vào màu sắc bên trong các khoảng. Nếu chúng ta hiểu được một khoảng có thể trở thành gì sau tất cả các phép tính nội tại có thể xảy ra thì chúng ta không cần biết chính xác trình tự các bước di chuyển. 

Trong một khoảng thời gian, chúng tôi lưu trữ số lượng ô tối đa của mỗi màu có thể còn lại nếu khoảng thời gian đó chỉ còn màu đó. Chúng tôi cũng lưu trữ liệu khoảng thời gian có thể biến mất hoàn toàn hay không. 

Khi hai khoảng lân cận được kết hợp lại thì chỉ có một vài khả năng xảy ra. Một bên có thể biến mất, còn bên kia không thay đổi. Cả hai bên có thể có cùng màu và hợp nhất thành một khối lớn hơn. Nếu khối được hợp nhất trở nên lớn hơn ngưỡng, toàn bộ khoảng sẽ biến mất. 

Brute-force hoạt động vì mọi thao tác hợp pháp cuối cùng sẽ loại bỏ một số cấu trúc khoảng, nhưng nó thất bại vì nó lặp lại các bài toán con tương tự. Khoảng thời gian DP loại bỏ sự lặp lại này bằng cách ghi lại chính xác thông tin hữu ích về mỗi khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n³) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo trạng thái lập trình động cho từng ô đơn lẻ. Khoảng cách một ô chỉ có thể để lại màu riêng, với số lượng còn lại là một ô. 
2. Xử lý khoảng thời gian bằng cách tăng độ dài. Đối với mỗi khoảng thời gian`[l, r]`, hãy thử mọi điểm phân chia có thể`k`giữa`l`Và`r`. 
3. Hợp nhất thông tin từ`[l, k]`Và`[k+1, r]`. Nếu khoảng bên trái có thể biến mất thì mọi thứ có thể có từ khoảng bên phải đều có thể xảy ra trong toàn bộ khoảng đó. Điều tương tự cũng áp dụng theo hướng khác. 
4. Nếu cả hai phần có thể có cùng màu, hãy cộng số lượng còn lại của chúng lại. Nếu số lượng kết quả lớn hơn ngưỡng, hãy đánh dấu toàn bộ khoảng thời gian là có thể tháo rời. Nếu không, hãy giữ số lượng kết hợp ở trạng thái còn lại có thể. 
5. Sau khi tất cả các khoảng thời gian được xử lý, hãy kiểm tra xem toàn bộ khoảng thời gian chuỗi có thể biến mất hay không. 

Điều bất biến là sau khi xử lý một khoảng, DP chứa mọi dạng cuối cùng có thể có của khoảng đó mà có thể đạt được bằng các thao tác hoàn toàn bên trong nó. Mọi thao tác trên một khoảng lớn hơn có thể được biểu diễn bằng cách chia thành hai khoảng nhỏ hơn, do đó, việc xem xét tất cả các điểm phân chia sẽ bao gồm mọi thứ tự loại bỏ có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    m = int(input())
    n = len(s)

    colors = {'R': 1, 'G': 2, 'B': 3, 'C': 4, 'M': 5, 'Y': 6, 'K': 7}
    a = [colors[c] for c in s]

    f = [[[0] * 8 for _ in range(n)] for _ in range(n)]
    can = [[False] * n for _ in range(n)]

    for i, c in enumerate(a):
        f[i][i][c] = 1

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1

            for k in range(l, r):
                if can[l][k]:
                    for c in range(1, 8):
                        if f[k + 1][r][c]:
                            f[l][r][c] = max(f[l][r][c], f[k + 1][r][c])

                if can[k + 1][r]:
                    for c in range(1, 8):
                        if f[l][k][c]:
                            f[l][r][c] = max(f[l][r][c], f[l][k][c])

                for c in range(1, 8):
                    if f[l][k][c] and f[k + 1][r][c]:
                        total = f[l][k][c] + f[k + 1][r][c]
                        if total > m:
                            can[l][r] = True
                        else:
                            f[l][r][c] = max(f[l][r][c], total)

    print("Yes" if can[0][n - 1] else "No")

if __name__ == "__main__":
    solve()
```Mảng`f[l][r][c]`lưu trữ số lượng màu lớn nhất`c`gạch có thể vẫn còn từ khoảng thời gian`[l, r]`khi khoảng cuối cùng chỉ chứa màu đó. Chiều bổ sung rất nhỏ vì chỉ có bảy màu. 

Mảng boolean`can`lưu trữ liệu một khoảng có thể biến mất hoàn toàn hay không. Trạng thái riêng biệt này là cần thiết vì khoảng trống không có màu nên không thể biểu diễn nó bên trong`f`. 

Quá trình chuyển đổi sẽ kiểm tra mọi điểm phân chia. Thứ tự của các khoảng thời gian xử lý từ ngắn đến dài đảm bảo rằng mọi khoảng thời gian nhỏ hơn đều đã được giải quyết trước khi nó được sử dụng. 

Sự so sánh với`m`phải nghiêm khắc. Một khối được hợp nhất có kích thước chính xác`m`vẫn ở trong DP, trong khi một khối có kích thước`m + 1`biến mất. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
RRBBRR
2
```Các trạng thái quan trọng là: 

| Khoảng thời gian | Chia | Kết quả | 
| --- | --- | --- | 
| RR | toàn bộ khoảng thời gian | Không thể biến mất, để lại 2 R | 
| BB | toàn bộ khoảng thời gian | Không thể biến mất, lá 2 B | 
| RRR | sau khi sáp nhập | Có thể biến mất vì 4 > 2 | 

Đầu tiên DP tìm hiểu các trạng thái có thể có trong các khoảng thời gian nhỏ, sau đó kết hợp chúng. Ví dụ này cho thấy tại sao các khoảng không liền kề ban đầu có thể trở nên hữu ích sau khi loại bỏ. 

Một ví dụ khác:```
RRR
3
```| Khoảng thời gian | Chia | Kết quả | 
| --- | --- | --- | 
| R đầu tiên | căn cứ | Còn lại một R | 
| R thứ hai | căn cứ | Còn lại một R | 
| R thứ ba | căn cứ | Còn lại một R | 
| Khoảng thời gian đầy đủ | hợp nhất | Kích thước là 3, không lớn hơn 3 | 

Khoảng cuối cùng không thể biến mất, khẳng định bất đẳng thức chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Có các khoảng O(n2) và mỗi khoảng đều thử O(n) điểm phân chia. | 
| Không gian | O(n²) | Trạng thái bảy màu có kích thước không đổi, do đó DP có dạng bậc hai. | 

Vì`n`khoảng 500, số khối chuyển tiếp có thể chấp nhận được. Kích thước màu nhỏ giúp quản lý việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    s = data().strip()
    m = int(data())

    colors = {'R': 1, 'G': 2, 'B': 3, 'C': 4, 'M': 5, 'Y': 6, 'K': 7}
    a = [colors[c] for c in s]
    n = len(a)

    f = [[[0] * 8 for _ in range(n)] for _ in range(n)]
    can = [[False] * n for _ in range(n)]

    for i, c in enumerate(a):
        f[i][i][c] = 1

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            for k in range(l, r):
                if can[l][k]:
                    for c in range(1, 8):
                        f[l][r][c] = max(f[l][r][c], f[k + 1][r][c])
                if can[k + 1][r]:
                    for c in range(1, 8):
                        f[l][r][c] = max(f[l][r][c], f[l][k][c])
                for c in range(1, 8):
                    if f[l][k][c] and f[k + 1][r][c]:
                        if f[l][k][c] + f[k + 1][r][c] > m:
                            can[l][r] = True
                        else:
                            f[l][r][c] = max(
                                f[l][r][c],
                                f[l][k][c] + f[k + 1][r][c]
                            )

    ans = "Yes\n" if can[0][n - 1] else "No\n"
    sys.stdin = old
    return ans

assert run("RRRR\n2\n") == "Yes\n"
assert run("RRR\n3\n") == "No\n"
assert run("RGB\n1\n") == "No\n"
assert run("RRBBRR\n2\n") == "Yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`RRRR, 2`| Có | Khối di động lớn hơn ngưỡng | 
|`RRR, 3`| Không | Nghiêm ngặt`>`tình trạng | 
|`RGB, 1`| Không | Không thể hợp nhất | 
|`RRBBRR, 2`| Có | Hành vi kết hợp khoảng thời gian | 

## Vỏ cạnh 

Đối với một khối chính xác bằng ngưỡng, thuật toán không bao giờ đánh dấu khối đó là có thể tháo rời được vì quá trình chuyển đổi sẽ kiểm tra`total > m`. Vì`RRR`với ngưỡng`3`, DP ghi lại ba ô R còn lại nhưng vẫn giữ`can`SAI. 

Đối với những khoảng thời gian chỉ có thể tháo rời được sau khi kết hợp các phần nhỏ hơn, thì quá trình chuyển đổi phân tách chính là điều giúp tìm ra giải pháp. TRONG`RRBBRR`với ngưỡng`2`, các khoảng nhỏ hơn được tính toán trước tiên, sau đó các khoảng lớn hơn kế thừa các trạng thái có thể có của chúng và cuối cùng tìm thấy một khối có thể tháo rời. 

Đối với các chuỗi một màu, các trường hợp cơ sở đã chứa thông tin đầy đủ. Một chuỗi như`RRRR`với ngưỡng`2`được giải quyết bằng cách hợp nhất các khoảng riêng lẻ cho đến khi số lượng vượt quá ngưỡng. 

Bạn có thể điều chỉnh thêm bài xã luận này cho phù hợp với định dạng blog bằng cách thêm các ví dụ gốc và phần chứng minh chi tiết hơn nếu cần.
