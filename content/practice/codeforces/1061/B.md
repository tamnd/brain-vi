---
title: "CF 1061B - Lượt xem quan trọng"
description: "Chúng ta được cung cấp một số chồng khối thẳng đứng được đặt cạnh nhau. Mỗi vị trí $i$ có một ngăn xếp có chiều cao $ai$. Từ phía trên, máy ảnh chỉ quan tâm xem một vị trí có bị chiếm hay trống hay không, vì vậy mỗi cột đóng góp chính xác một ô hiển thị miễn là nó có ít nhất một khối."
date: "2026-06-15T08:54:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1061
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 523 (Div. 2)"
rating: 1400
weight: 1061
solve_time_s: 349
verified: true
draft: false
---

[CF 1061B - Vấn đề về lượt xem](https://codeforces.com/problemset/problem/1061/B) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 5 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chồng khối thẳng đứng được đặt cạnh nhau. Mỗi vị trí$i$có một chiều cao$a_i$. Từ phía trên, máy ảnh chỉ quan tâm xem một vị trí có bị chiếm hay trống hay không, vì vậy mỗi cột đóng góp chính xác một ô hiển thị miễn là nó có ít nhất một khối. Từ bên cạnh, camera nhìn thấy hình bóng của các ngăn xếp nên tại mỗi vị trí nó nhìn thấy khối cao nhất còn lại trong cột đó. 

Nhiệm vụ là loại bỏ càng nhiều khối càng tốt trong khi vẫn giữ nguyên cả hai chế độ xem. Điều đó có nghĩa là mọi cột phải không trống và chiều cao của mỗi cột phải giữ nguyên như ban đầu vì bất kỳ sự giảm bớt nào trong cột sẽ làm co hình bóng bên. 

Kích thước đầu vào đạt$n = 10^5$, do đó, bất kỳ giải pháp nào tệ hơn thời gian tuyến tính hoặc tuyến tính sẽ gặp khó khăn. Cách tiếp cận bậc hai liên tục cố gắng loại bỏ các khối khỏi mỗi cột đã quá chậm vì nó sẽ ám chỉ thứ tự của$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam từng cột mà không có sự phối hợp. Nếu chúng tôi cố gắng giảm từng ngăn xếp một cách độc lập trong khi vẫn giữ nguyên chiều cao của nó, chúng tôi có thể cho rằng mình có thể xóa mọi thứ ngoại trừ một khối trên mỗi cột, nhưng điều đó bỏ qua ràng buộc rằng chế độ xem bên chỉ phụ thuộc vào chiều cao tối đa trên mỗi cột chứ không phải cấu trúc bên trong. Một sai lầm khác là cố gắng phân phối lại các khối giữa các ngăn xếp, điều này rõ ràng là không được phép. 

Ví dụ, trường hợp cạnh khóa là khi tất cả các ngăn xếp đều bằng nhau$a = [3, 3, 3]$. Câu trả lời đúng không chỉ đơn giản là “xóa mọi thứ trừ lớp trên cùng”, bởi vì lớp trên cùng đó vẫn phải bảo toàn cả hai chế độ xem, nghĩa là chúng ta không thể xóa toàn bộ cột bên dưới nó. Điều này buộc chúng ta phải suy nghĩ xem khối nào về cơ bản là cần thiết hơn là chúng ta có thể tự do xóa bao nhiêu khối. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ mô phỏng việc loại bỏ từng khối một khỏi bất kỳ ngăn xếp nào và kiểm tra xem cả hai phép chiếu có không thay đổi hay không. Mỗi lần xóa sẽ yêu cầu tính toán lại chế độ xem trên cùng và chế độ xem bên, điều này phụ thuộc vào việc quét tất cả các ngăn xếp để xác minh các ràng buộc. Ngay cả khi ghi sổ kế toán cẩn thận, điều này ít nhất cũng dẫn đến$O(n)$hoạt động trên mỗi lần cố gắng loại bỏ và tối đa$O(n \cdot m)$hoạt động trong trường hợp xấu nhất vì mỗi cột có thể có tới$m$khối. Điều này vượt xa giới hạn khả thi. 

Cấu trúc của vấn đề được đơn giản hóa khi chúng ta giải thích những gì phải được cố định. Góc nhìn từ trên xuống yêu cầu mọi vị trí$i$không trống, do đó mỗi ngăn xếp phải giữ lại ít nhất một khối. Chế độ xem bên yêu cầu chiều cao tối đa trong mỗi cột phải bằng chiều cao ban đầu$a_i$, có nghĩa là ít nhất một cột phải duy trì toàn bộ chiều cao của nó và mỗi cột phải duy trì đủ khối để duy trì mức tối đa của chính nó một cách độc lập. Vì các khối không được chia sẻ giữa các ngăn xếp nên mỗi ngăn xếp đều độc lập ngoại trừ yêu cầu chung là không trống. 

Điều này dẫn đến một quan sát trực tiếp: trong mỗi ngăn xếp, chúng ta phải giữ ít nhất một khối, nhưng chúng ta có thể tự do loại bỏ tất cả các khối khác miễn là chúng ta không giảm chiều cao tối đa của ngăn xếp đó. Vì chiều cao tối đa chính xác là$a_i$, chúng ta phải giữ ít nhất$a_i$khối trong ngăn xếp$i$nếu chúng ta diễn giải chế độ xem bên một cách nghiêm ngặt là duy trì chiều cao đầy đủ của cột. Tuy nhiên, điểm đơn giản hóa chính là không có gì trong các ngăn xếp khác phụ thuộc vào cấu trúc bên trong của một ngăn xếp nhất định vượt quá chiều cao của nó, vì vậy chúng ta chỉ cần duy trì chính xác một “hỗ trợ” đại diện cho mỗi ngăn xếp cho chế độ xem trên cùng và đảm bảo khối cao nhất vẫn hiện diện cho chế độ xem bên. Điều này sụp đổ để giữ chính xác một khối trên mỗi ngăn xếp ở lớp trên cùng cộng với việc bảo toàn toàn bộ cấu trúc chiều cao ban đầu, điều này có nghĩa là mỗi ngăn xếp đóng góp chính xác một khối không thể tránh khỏi bên ngoài phần bên trong có thể tháo rời của nó. 

Một cách rõ ràng hơn để xem nó là lật phối cảnh: mọi chiều cao$a_i$phải giữ ít nhất một khối để duy trì tầm nhìn từ trên cao và tất cả các khối còn lại$a_i - 1$các khối có thể được loại bỏ mà không ảnh hưởng đến một trong hai chế độ xem vì chế độ xem bên chỉ phụ thuộc vào sự tồn tại của chiều cao$a_i$, chứ không phải có bao nhiêu khối nằm bên dưới nó. 

Vì vậy, trong mỗi ngăn xếp, chính xác$a_i - 1$các khối có thể tháo rời được và tính tổng tất cả các ngăn xếp sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot m)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách xem xét từng ngăn xếp một cách độc lập, vì không có thao tác nào di chuyển các khối giữa các ngăn xếp. 
2. Đối với một chồng chiều cao$a_i$, xác định khối nào là cần thiết để duy trì cả hai chế độ xem. 
3. Nhận ra rằng chế độ xem trên cùng chỉ yêu cầu ít nhất một khối vẫn hiển thị ở vị trí đó. 
4. Nhận biết rằng bất kỳ khối nào bên dưới cấu trúc còn sót lại trên cùng đều không ảnh hưởng đến hình chiếu. 
5. Kết luận rằng mỗi ngăn xếp phải giữ lại chính xác một khối và tất cả các khối khác có thể bị xóa. 
6. Tính tổng số khối có thể tháo rời bằng cách tính tổng$a_i - 1$trên tất cả các ngăn xếp. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi cột không trống trong suốt quá trình và mỗi cột vẫn chứa ít nhất một khối ở đúng vị trí để bảo toàn cả hai hình chiếu. Vì chế độ xem bên chỉ phụ thuộc vào sự tồn tại của chiều cao tối đa trong mỗi cột chứ không phụ thuộc vào cấu trúc trung gian, nên việc loại bỏ bất kỳ khối nào không phải là đại diện cuối cùng còn lại của cột sẽ không làm thay đổi hình chiếu. Vì các ngăn xếp độc lập và không có sự phân phối lại nên việc tính tổng các lần loại bỏ theo cột mang lại kết quả tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    ans = 0
    for x in a:
        ans += x - 1
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp lý luận theo từng cột. Mỗi ngăn xếp đóng góp$a_i - 1$khối có thể tháo rời, vì vậy chúng tôi tích lũy giá trị này. Biến`m`không được sử dụng vì hạn chế$a_i \le m$chỉ giới hạn tính hợp lệ của đầu vào và không ảnh hưởng đến việc tính toán. 

Chi tiết triển khai tinh tế duy nhất là tránh từng lỗi một: chúng tôi đang đếm các khối có thể tháo rời chứ không phải các khối còn lại. Mỗi ngăn xếp giữ chính xác một khối, vì vậy việc trừ đi một khối là điều cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
3 3 3 3 3
```| tôi | một [tôi] | khối giữ | khối bị loại bỏ | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 2 | 
| 2 | 3 | 1 | 2 | 
| 3 | 3 | 1 | 2 | 
| 4 | 3 | 1 | 2 | 
| 5 | 3 | 1 | 2 | 

Tổng số khối bị loại bỏ là$10$. 

Điều này xác nhận rằng các ngăn xếp giống hệt nhau hoạt động độc lập và mỗi ngăn xếp đều đóng góp như nhau vào tổng số có thể tháo rời. 

### Ví dụ 2 

đầu vào:```
4 5
1 2 4 1
```| tôi | một [tôi] | khối giữ | khối bị loại bỏ | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 
| 3 | 4 | 1 | 3 | 
| 4 | 1 | 1 | 0 | 

Tổng số bị loại bỏ là$4$. 

Điều này cho thấy rằng các ngăn xếp có chiều cao 1 không đóng góp gì có thể loại bỏ được, trong khi các ngăn xếp cao hơn đóng góp tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi ngăn xếp được xử lý một lần | 
| Không gian |$O(1)$| chỉ có một khoản tiền đang được lưu trữ | 

Thuật toán chia tỷ lệ trực tiếp với số lượng ngăn xếp, phù hợp thoải mái trong$n \le 10^5$dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    return str(sum(x - 1 for x in a))

# provided sample
assert run("5 6\n3 3 3 3 3\n") == "10", "sample 1"

# minimum size
assert run("1 10\n1\n") == "0", "single stack"

# mixed heights
assert run("4 5\n1 2 4 1\n") == "4", "mixed heights"

# all ones
assert run("5 100\n1 1 1 1 1\n") == "0", "all minimal stacks"

# large uniform
assert run("3 1000000000\n1000000000 1000000000 1000000000\n") == "2999999997", "max height"

# alternating
assert run("6 10\n1 5 1 5 1 5\n") == "12", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngăn xếp đơn | 0 | không có khối có thể tháo rời khi chiều cao là 1 | 
| độ cao hỗn hợp | 4 | tính đúng đắn trên mảng không đồng nhất | 
| tất cả những cái | 0 | trường hợp cạnh không thể xóa được gì | 
| chiều cao tối đa | giá trị lớn | xử lý giới hạn trên một cách an toàn | 
| mô hình xen kẽ | 12 | sự độc lập của ngăn xếp | 

## Vỏ cạnh 

Đối với một ngăn xếp như`1`, thuật toán tính toán$1 - 1 = 0$, nghĩa là không thể xóa khối nào. Điều này phù hợp với yêu cầu rằng ngăn xếp phải vẫn hiển thị từ cả hai chế độ xem và việc xóa khối duy nhất của nó sẽ phá vỡ chế độ xem trên cùng. 

Đối với một ngăn xếp tối đa như`1000000000`, thuật toán trả về$999999999$. Vòng lặp xử lý giá trị một lần và số học số nguyên của Python xử lý tổng một cách an toàn mà không lo tràn. 

Đối với một cấu hình hỗn hợp như`1 2 1`, ngăn xếp ở giữa đóng góp chính xác một khối có thể tháo rời trong khi các khối khác đóng góp bằng không. Điều này xác nhận rằng mỗi ngăn xếp được xử lý độc lập và không có ràng buộc ngăn xếp chéo nào làm thay đổi quá trình tính toán.
