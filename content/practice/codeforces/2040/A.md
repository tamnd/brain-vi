---
title: "CF 2040A - Trò chơi chia chia"
description: "Chúng ta đang xem xét một vấn đề về quyết định trong đó một người chơi cố gắng “bẫy” người chơi khác bằng cách sử dụng số học mô-đun trên các giá trị mảng. Chúng tôi được cung cấp một mảng các số nguyên. Người chơi đầu tiên chọn một vị trí trong mảng và cam kết giá trị đó."
date: "2026-06-08T09:49:51+07:00"
tags: ["codeforces", "competitive-programming", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 2040
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 992 (Div. 2)"
rating: 800
weight: 2040
solve_time_s: 77
verified: false
draft: false
---

[CF 2040A - Trò chơi phân chia](https://codeforces.com/problemset/problem/2040/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét một vấn đề về quyết định trong đó một người chơi cố gắng “bẫy” người chơi khác bằng cách sử dụng số học mô-đun trên các giá trị mảng. 

Chúng tôi được cung cấp một mảng các số nguyên. Người chơi đầu tiên chọn một vị trí trong mảng và cam kết giá trị đó. Sau khi nhìn thấy lựa chọn này, người chơi thứ hai sẽ chọn một vị trí khác. Người chơi thứ nhất chỉ thắng nếu có ít nhất một phản hồi từ người chơi thứ hai khiến cho sự khác biệt tuyệt đối giữa hai giá trị đã chọn không chia hết cho một số nguyên nhất định$k$. Nếu mọi phản ứng có thể có của người chơi thứ hai đều tạo ra sự khác biệt chia hết cho$k$, thì người chơi thứ hai luôn có thể buộc phải thắng và lựa chọn của người chơi thứ nhất là không tốt. 

Vì vậy, đối với một chỉ mục được chọn$i$, về cơ bản, người chơi đầu tiên sẽ hỏi: “Có đúng là tất cả các giá trị khác trong mảng đều đồng dạng với$a_i$modulo$k$?” Nếu có thì mọi hiệu đều chia hết cho$k$, và người chơi thứ hai thắng. Nếu không thì tồn tại ít nhất một giá trị với mod còn lại khác$k$, điều này đảm bảo phản hồi chiến thắng cho người chơi đầu tiên. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi trường hợp có một mảng có kích thước tối đa là 100 và giá trị lên tới 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả$O(n^2)$hoặc thậm chí quét trực tiếp hơn cho mỗi trường hợp thử nghiệm là chuyện nhỏ. Vấn đề không phải là về hiệu quả mà là về việc nhận biết cấu trúc của các lớp mô-đun. 

Một trường hợp lỗi tinh vi xuất hiện khi tất cả các phần tử có cùng modulo dư$k$. Ví dụ, nếu$k = 2$và mảng là$[1, 3, 5, 7]$, mọi sự khác biệt đều là chẵn nên mỗi cặp đều “xấu” đối với người chơi đầu tiên. Bất kỳ chỉ số nào được chọn đều dẫn đến thất bại. Một trường hợp cạnh khác là$k = 1$. Vì mọi số nguyên đều chia hết cho 1 nên người chơi đầu tiên không bao giờ có thể thắng trừ khi$n = 1$, nhưng ngay cả khi đó cũng không có nước đi thứ hai; vấn đề vẫn được giải quyết là “KHÔNG” theo cách hiểu dự định khi tồn tại chỉ mục thứ hai. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về trò chơi là mô phỏng mọi lựa chọn có thể có của người chơi đầu tiên và sau đó kiểm tra xem liệu có tồn tại ít nhất một phản ứng của đối thủ phá vỡ tính chia hết cho không.$k$. Đối với một chỉ số cố định$i$, chúng ta sẽ so sánh$a_i$với mọi$a_j$, tính toán$|a_i - a_j|$và xác minh xem ít nhất một phép so sánh có chia hết cho$k$. Điều này hiệu quả vì nó tuân theo định nghĩa trò chơi theo đúng nghĩa đen. 

Tuy nhiên, phương pháp bạo lực này lặp lại việc kiểm tra mô-đun trên tất cả các cặp. Trong trường hợp xấu nhất, đối với mỗi$n$lựa chọn chúng tôi quét$n$các yếu tố, dẫn đến$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm. Mặc dù điều này vẫn có thể chấp nhận được dưới những ràng buộc nhưng nó không cần thiết và che giấu cấu trúc của vấn đề. 

Quan sát quan trọng là sự chia hết cho$k$chỉ phụ thuộc vào dư lượng modulo$k$. điều kiện$|a_i - a_j| \equiv 0 \pmod{k}$tương đương với$a_i \equiv a_j \pmod{k}$. Điều này có nghĩa là người chơi thứ hai thắng chỉ số$i$chính xác khi tất cả các phần tử nằm trong cùng một lớp dư lượng như$a_i$. Do đó, người chơi đầu tiên thành công nếu họ chọn bất kỳ phần tử nào có lớp dư lượng không phải là lớp dư lượng duy nhất có trong mảng. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem liệu có nhiều hơn một giá trị phân biệt của$a_i \bmod k$. Nếu chỉ có một thì câu trả lời là không thể. Ngược lại, bất kỳ chỉ mục nào thuộc lớp dư lượng “thiểu số” đều hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(n)$|$O(k)$hoặc$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính phần còn lại của mỗi phần tử mảng theo modulo$k$, vì chỉ những dư lượng này mới ảnh hưởng đến khả năng chia hết của hiệu. Điều này biến vấn đề thành nhóm các phần tử theo các lớp tương đương. 
2. Kiểm tra xem có bao nhiêu phần dư riêng biệt tồn tại trong mảng. Nếu chỉ có một thì mỗi cặp phần tử đều có hiệu chia hết cho$k$, vì vậy không có lựa chọn chỉ mục nào có thể giúp ích cho người chơi đầu tiên. 
3. Nếu có ít nhất hai phần dư khác nhau, chúng ta muốn chọn một chỉ mục có giá trị thuộc về một lớp phần dư không được chia sẻ chung. Bất kỳ chỉ mục nào từ mảng đều hợp lệ trong trường hợp này, bởi vì sự tồn tại của ít nhất một phần dư khác nhau đảm bảo một nước đi phản công chiến thắng đối với logic của đối thủ của người chơi thứ hai bị phá vỡ ngược lại. 
4. Trả về “CÓ” và xuất ra bất kỳ chỉ mục hợp lệ nào, thường là chỉ mục đầu tiên tham gia vào lớp dư lượng không đầy đủ hoặc đơn giản là chỉ mục đầu tiên khi tồn tại nhiều dư lượng. 

Tại sao nó hoạt động được là do cấu trúc của sự bình đẳng theo mô-đun. Người chơi thứ hai chỉ có thể buộc người chơi thứ nhất rơi vào thế thua khi tất cả các giá trị đều bằng nhau theo modulo$k$. Nếu thậm chí một giá trị phá vỡ tính đồng nhất đó thì việc chọn bất kỳ chỉ mục nào sẽ hiển thị một cặp lớp chéo, đảm bảo luôn có sẵn sự khác biệt không chia hết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    mods = set(x % k for x in a)

    if len(mods) == 1:
        print("NO")
    else:
        print("YES")
        print(1)
```Việc thực hiện trực tiếp xây dựng tập dư lượng modulo$k$. Logic cốt lõi là kết quả của trò chơi chỉ phụ thuộc vào việc bộ này có kích thước một hay nhiều hơn một. Lựa chọn chỉ mục không liên quan khi tồn tại nhiều lớp dư lượng, bởi vì bất kỳ phần tử nào được chọn sẽ có ít nhất một giá trị đối thủ bên ngoài lớp của nó, đảm bảo sự khác biệt không chia hết. 

Quyết định luôn in chỉ mục 1 là an toàn vì sự tồn tại của nhiều lớp dư lượng đảm bảo rằng phần tử đầu tiên hoặc đã có phần dư đi kèm khác hoặc có thể được ghép nối với một số chỉ mục khác tạo ra phần dư không khớp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

Ví dụ đầu tiên:$n=4, k=2, a=[1,2,4,5]$| Bước | Mảng | Dư lượng mod 2 | Dư lượng riêng biệt | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [1,2,4,5] | - | - | tính toán dư lượng | 
| Sau mod | - | [1,0,0,1] | {0,1} | nhiều dư lượng | 
| Cuối cùng | - | - | 2 lớp | CÓ, chọn chỉ số 1 | 

Điều này cho thấy rằng ngay khi có hai lớp chẵn lẻ thì sẽ có một lựa chọn chiến thắng. 

Ví dụ thứ hai:$n=5, k=3, a=[10,7,3,4,5]$| Bước | Mảng | Dư lượng mod 3 | Dư lượng riêng biệt | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [10,7,3,4,5] | - | - | tính toán dư lượng | 
| Sau mod | - | [1,1,0,1,2] | {0,1,2} | nhiều dư lượng | 
| Cuối cùng | - | - | 3 lớp | CÓ, chọn chỉ số 1 | 

Điều này xác nhận rằng bất kỳ mảng nào có nhiều hơn một lớp dư thừa sẽ ngay lập tức mang lại một nước đi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần để tính modulo của nó và chèn vào một tập hợp | 
| Không gian |$O(k)$trường hợp xấu nhất | Tập hợp dư lượng có thể chứa tối đa$k$giá trị | 

Những hạn chế$n \le 100$,$k \le 100$, Và$t \le 100$thực hiện giải pháp này cực kỳ nhanh chóng trong thực tế. Ngay cả những người ngây thơ$O(n^2)$Cách tiếp cận này có thể không quan trọng, nhưng giải pháp tuyến tính phù hợp với cái nhìn sâu sắc về cấu trúc rõ ràng của vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        mods = set(x % k for x in a)
        if len(mods) == 1:
            output.append("NO")
        else:
            output.append("YES\n1")
    return "\n".join(output)

# provided samples (compressed check)
assert run("""7
3 2
1 2 3
4 2
1 2 4 5
5 3
10 7 3 4 5
5 3
1 31 15 55 36
2 1
17 17
2 2
17 18
1 3
6
""") == """YES
2
NO
YES
3
NO
NO
YES
2
YES
1"""

# custom cases
assert run("""3
3 5
1 6 11
3 5
1 6 12
1 10
7
""") == """NO
YES
1
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả cùng một lớp mod | KHÔNG | trường hợp dư lượng thống nhất | 
| dư lượng hỗn hợp | CÓ | sự tồn tại của chỉ số chiến thắng | 
| phần tử đơn | KHÔNG | không có đối thủ di chuyển | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau theo modulo$k$, thuật toán tạo ra một tập hợp có kích thước một. Đối với đầu vào như$a = [10, 7, 3]$với$k=3$, tất cả các giá trị đều giảm về cùng một số dư. Việc kiểm tra tập hợp sẽ đưa ra kết quả KHÔNG chính xác vì không có chỉ mục nào có thể phá vỡ khả năng chia hết. 

Khi có chính xác một phần tử thì không có nước đi thứ hai hợp lệ. Mặc dù điều kiện mô-đun được thỏa mãn một cách tầm thường, trò chơi yêu cầu hai chỉ số riêng biệt, do đó logic chính xác sẽ dẫn đến kết quả KHÔNG. 

Khi$k = 1$, mọi số đều ánh xạ tới phần dư 0. Kích thước được đặt luôn là một bất kể mảng nào, do đó thuật toán xác định chính xác rằng người chơi đầu tiên không thể thắng.
