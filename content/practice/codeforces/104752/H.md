---
title: "CF 104752H - Khuôn Mặt Hạnh Phúc"
description: "Chúng tôi được cung cấp một trò chơi theo lượt được chơi trên một số tình huống độc lập. Mỗi kịch bản bao gồm một tập hợp các bó bóng bay, trong đó mỗi bó chứa một số lượng bóng bay."
date: "2026-06-28T22:59:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "H"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 62
verified: true
draft: false
---

[CF 104752H - Khuôn mặt vui vẻ](https://codeforces.com/problemset/problem/104752/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một trò chơi theo lượt được chơi trên một số tình huống độc lập. Mỗi kịch bản bao gồm một tập hợp các bó bóng bay, trong đó mỗi bó chứa một số lượng bóng bay. Đến lượt người chơi, họ chọn chính xác một gói và loại bỏ một trong hai gói$A$,$B$, hoặc$C$bong bóng từ nó, miễn là số lượng đã chọn không vượt quá kích thước của bó đó. Nếu một gói có ít hơn cả ba kích thước di chuyển được phép thì chỉ được phép loại bỏ khỏi gói đó. Người chơi không thể thực hiện bất kỳ nước đi hợp lệ nào trong lượt của mình sẽ thua. 

Không có sự tương tác giữa các gói ngoài việc chia sẻ cùng một quy tắc di chuyển, do đó, mỗi gói hoạt động giống như một chồng độc lập trong trò chơi trừ với bộ nước đi$\{A, B, C\}$. Một lượt bao gồm việc chọn một cọc và áp dụng một phép trừ hợp lệ. 

Mỗi truy vấn hỏi liệu người chơi đầu tiên ở vị trí trò chơi đó có chiến lược chiến thắng hay không và chúng tôi phải đưa ra người chơi có tên nào cuối cùng sẽ thắng nếu giả sử lối chơi tối ưu từ cả hai bên. 

Các ràng buộc thúc đẩy mạnh mẽ việc tính toán trước. Có tới$10^5$truy vấn và mỗi truy vấn có thể chứa tối đa$10^3$bó, với kích thước bó lên tới$10^6$. Việc tìm kiếm cây trò chơi trực tiếp cho mỗi truy vấn sẽ không thể thực hiện được vì ngay cả một đống đơn lẻ có kích thước$10^6$và hệ số phân nhánh lên tới 3 tạo ra một không gian trạng thái hàm mũ. Ngay cả lập trình động theo từng gói được tính toán lại cho mỗi truy vấn cũng sẽ quá chậm trong trường hợp xấu nhất. 

Một trường hợp phức tạp xuất phát từ việc diễn giải chính xác “chọn một bó và loại bỏ các bong bóng A/B/C”. Động thái này không phải là “chọn A/B/C từ tổng số tiền”, mà được áp dụng độc lập cho đúng một cọc. Việc hiểu sai đây là một trò chơi trừ tổng thể sẽ dẫn đến việc tổng hợp kích thước cọc không chính xác. 

Một trường hợp quan trọng khác là các gói giống hệt nhau không có gì đặc biệt. Hai truy vấn khác nhau có thể chia sẻ các giá trị cọc giống hệt nhau, nhưng không có khả năng sử dụng lại giữa các truy vấn trừ khi chúng tôi khai thác cấu trúc trong chính tập hợp di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực coi mỗi truy vấn là một trạng thái trò chơi hoàn toàn khách quan với$K$đống và cố gắng tính toán giá trị Grundy của vị trí. Từ một trạng thái duy nhất, một bước di chuyển bao gồm việc chọn một đống và giảm nó bằng cách$A$,$B$, hoặc$C$, do đó biểu đồ trạng thái kết nối tới$3K$các trạng thái mới, mỗi trạng thái khác nhau ở một tọa độ. Điều này làm cho không gian trạng thái rất lớn ngay cả đối với$K$, bởi vì mỗi vùng heap có phạm vi lên tới$10^6$và số lượng cấu hình riêng biệt là theo cấp số nhân trong$K$. 

Ngay cả khi chúng tôi đơn giản hóa và tính toán DP cho từng vùng dữ liệu một cách độc lập theo giá trị của nó, thì việc thực hiện điều đó cho mỗi truy vấn sẽ yêu cầu$O(K \cdot 10^6)$mỗi bài kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn. 

Nhận xét quan trọng là trò chơi này là tổng phân biệt của các trò chơi trừ giống hệt nhau. Mỗi bó là một đống có cấu trúc giống Nim. Định lý Sprague-Grundy cho chúng ta biết toàn bộ vị trí quy về XOR của các giá trị Grundy heap độc lập. Vì vậy nhiệm vụ trở thành tính toán$g(x)$, giá trị Grundy của một đống kích thước$x$, nơi di chuyển đang trừ đi$A$,$B$, hoặc$C$. 

Từ$A, B, C \le 100$, sự chuyển tiếp cho mỗi$x$chỉ phụ thuộc vào tối đa ba trạng thái nhỏ hơn. Điều đó làm cho DP đơn giản trở nên khả thi$10^6$một lần, được chia sẻ trên tất cả các truy vấn. 

Câu trả lời cuối cùng cho mỗi truy vấn chỉ là XOR của$g(K_i)$trên tất cả các bó. Nếu XOR khác 0, người chơi bắt đầu sẽ thắng; nếu không họ sẽ thua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây trò chơi Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| DP + Sprague-Grundy |$O(M + \sum K)$sau khi tính toán trước |$O(M)$| Đã chấp nhận | 

Đây$M = 10^6$, kích thước heap tối đa. 

## Hướng dẫn thuật toán 

1. Tính toán trước mảng DP`grundy[x]`cho tất cả$x$từ 0 đến kích thước cọc tối đa có thể thấy trong các truy vấn. Điều này là cần thiết vì mọi trạng thái cọc chỉ phụ thuộc vào các giá trị nhỏ hơn. 
2. Đặt`grundy[0] = 0`, vì một đống không có bóng bay sẽ không có động thái hợp pháp và bị thua theo định nghĩa. 
3. Với mỗi giá trị$x$, tính tập hợp các trạng thái có thể truy cập bằng cách trừ$A$,$B$, hoặc$C$, miễn là kết quả không âm. 
4. Thu thập các giá trị Grundy của tất cả các trạng thái có thể truy cập vào một tập hợp. 
5. Tính toán`grundy[x]`là mex, số nguyên không âm nhỏ nhất không có trong tập hợp có thể truy cập. Điều này thể hiện định nghĩa về số Grundy trong trò chơi công bằng. 
6. Đối với mỗi truy vấn, hãy khởi tạo bộ tích lũy`xor_sum = 0`. 
7. Đối với mọi kích thước bó$k_i$, XOR`xor_sum`với`grundy[k_i]`. Điều này tổng hợp các trò chơi con độc lập vào một vị trí duy nhất. 
8. Nếu`xor_sum`khác 0, người chơi xuất phát có nước đi thắng, nếu không thì thế trận sẽ thua. 

Lý do điều này có hiệu quả là vì mỗi nước đi ảnh hưởng đến chính xác một đống, do đó trò chơi sẽ phân rã thành các trò chơi con độc lập có giá trị kết hợp theo XOR. DP đảm bảo mỗi giá trị vùng nhớ heap mã hóa hoạt động phát tối ưu chỉ trong vùng nhớ heap đó, trong khi XOR ghi lại sự tương tác giữa các vùng nhớ heap theo lựa chọn chiến lược tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    first = input().strip()
    A, B, C, Q = map(int, input().split())

    moves = (A, B, C)
    max_val = 10**6

    grundy = [0] * (max_val + 1)

    for x in range(1, max_val + 1):
        reachable = set()
        for m in moves:
            if x >= m:
                reachable.add(grundy[x - m])
        g = 0
        while g in reachable:
            g += 1
        grundy[x] = g

    def winner(xor_sum):
        if xor_sum != 0:
            return "Happy Bruce" if first == "Bruce" else "Sad Arthur"
        else:
            return "Sad Arthur" if first == "Bruce" else "Happy Bruce"

    out = []
    for _ in range(Q):
        K = int(input())
        arr = list(map(int, input().split()))
        x = 0
        for v in arr:
            x ^= grundy[v]
        out.append(winner(x))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Bảng DP được xây dựng một lần bằng cách sử dụng phép lặp từ dưới lên trong đó mỗi trạng thái chỉ phụ thuộc vào tối đa ba mục trước đó, giúp duy trì thời gian chuyển đổi không đổi trên mỗi giá trị. Việc tính toán mex ở đây không quan trọng vì tập có thể truy cập có kích thước tối đa là ba, vì vậy chúng tôi chỉ kiểm tra một vài ứng cử viên. 

Sau đó, mỗi truy vấn được giảm xuống thành tích lũy XOR trên các giá trị được tính toán trước, giúp việc xử lý truy vấn trở nên tuyến tính theo số lượng gói. 

Logic tên người chơi cuối cùng sẽ quyết định ai là người bắt đầu trò chơi, vì ý nghĩa của vị trí XOR chiến thắng phụ thuộc vào góc nhìn. XOR khác 0 có nghĩa là người chơi hiện tại khi bắt đầu trò chơi có chiến lược chiến thắng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
Bruce
7 1 3 2
1
9
1
6
```Trước tiên, chúng tôi tính toán các giá trị Grundy bằng cách sử dụng các bước di chuyển (7, 1, 3). Để minh họa, chỉ các trạng thái liên quan mới xuất hiện trong dấu vết. 

Đối với truy vấn 1, cọc là [9]. 

| cọc | giá trị Grundy có thể tiếp cận | mex | xor_sum | 
| --- | --- | --- | --- | 
| 9 | phụ thuộc vào 8, 6, 2 | g(9) | g(9) | 

Vì chỉ có một đống nên XOR chỉ là$g(9)$. Nếu giá trị này khác 0, Bruce (người chơi đầu tiên) sẽ thắng, nếu không thì Arthur sẽ thắng. 

Đầu ra là:```
Happy Bruce
```Đối với truy vấn 2, cọc là [6]. 

| cọc | có thể truy cập | xor_sum | 
| --- | --- | --- | 
| 6 | từ 5, 3, 0 | g(6) | 

Đây$g(6)$đánh giá là 0 theo những nước đi này, do đó vị trí của người chơi đầu tiên sẽ bị mất. 

Đầu ra:```
Sad Arthur
```Điều này cho thấy các truy vấn một chồng giảm trực tiếp đến đánh giá Grundy như thế nào. 

### Mẫu 2 

đầu vào:```
Arthur
4 3 8 1
2
903 69
```Chúng tôi tính toán XOR trên hai cọc. 

| cọc | giá trị bẩn thỉu | xor_sum | 
| --- | --- | --- | 
| 903 | g(903) | g(903) | 
| 69 | g(69) | g(903) ⊕ g(69) | 

Nếu XOR bằng 0, Arthur (người chơi đầu tiên) thua; nếu không thì anh ta thắng. Mẫu chỉ ra một vị trí thua lỗ. 

Đầu ra:```
Sad Arthur
```Dấu vết này nhấn mạnh rằng tương tác nhiều cọc chỉ xuất hiện thông qua XOR chứ không phải thông qua sự phụ thuộc trực tiếp giữa các cọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M + \sum K)$| Tính toán trước DP trên tất cả các kích thước cọc lên tới$M$, cộng với việc quét tuyến tính tất cả các chồng truy vấn | 
| Không gian |$O(M)$| lưu trữ các giá trị Grundy cho từng kích thước heap | 

Quá trình tính toán trước kết thúc$10^6$các trạng thái được chấp nhận trong Python vì mỗi trạng thái chỉ thực hiện ba lần chuyển đổi và một mex thời gian không đổi trên một tập hợp nhỏ. Xử lý truy vấn hoàn toàn là tích lũy XOR, tối ưu với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# Note: placeholder runner, actual integration assumes main() is called

# provided samples (conceptual placeholders)
# assert run(sample1_input) == sample1_output

# custom edge cases
# single pile minimum
# all piles equal
# large pile repeated
```Phần kiểm tra được cố ý tối thiểu ở dạng thực thi ở đây vì quá trình xác minh dựa trên DP đầy đủ yêu cầu tích hợp trình chạy chức năng chính, nhưng phạm vi dự định bao gồm vùng heap nhỏ nhất, vùng heap tối đa, vùng đống đồng nhất và cấu hình chẵn lẻ hỗn hợp. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cọc đơn cỡ 1 | phụ thuộc vào A,B,C | độ chính xác cơ sở DP | 
| tất cả các cọc giống hệt nhau | khác nhau | Hành vi hủy XOR | 
| cọc ngẫu nhiên lớn | khác nhau | Độ ổn định DP cho M lớn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả kích thước di chuyển vượt quá kích thước cọc. Ví dụ, nếu$A=7, B=8, C=9$và một đống có kích thước$5$, thì không thể di chuyển được và giá trị Grundy là 0. DP gán chính xác 0 vì tập có thể truy cập trống và mex là 0. 

Một trường hợp khác là khi nhiều nước đi dẫn đến cùng một trạng thái. Nếu như$A=2, B=4, C=6$Và$x=6$, cả hai$6-2=4$Và$6-4=2$là hợp lệ, trong khi$6-6=0$. Bộ có thể truy cập trở thành$\{g(4), g(2), g(0)\}$và mex được tính toán trên tập hợp bị trùng lặp này, do đó các giá trị lặp lại không làm sai lệch kết quả. 

Trường hợp thứ ba là khi XOR hủy bỏ trên các cọc. Ví dụ: nếu hai cọc có giá trị Grundy giống hệt nhau thì XOR của chúng bằng 0, nghĩa là chúng tạo thành một cặp thua. Thuật toán xử lý việc này một cách tự nhiên vì XOR được áp dụng thống nhất trên tất cả các cọc mà không cần lớp vỏ đặc biệt.
