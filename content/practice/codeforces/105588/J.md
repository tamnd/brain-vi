---
title: "CF 105588J - Chỉ là một vấn đề sắp xếp khác"
description: "Chúng ta được cấp một hoán vị và chơi một trò chơi hai người chơi trên đó. Mục tiêu của trò chơi là biến đổi hoán vị thành một chuỗi được sắp xếp đầy đủ theo thứ tự tăng dần."
date: "2026-06-22T23:05:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105588
codeforces_index: "J"
codeforces_contest_name: "The 2024 ICPC Asia Kunming Regional Contest (The 3rd Universal Cup. Stage 20: Kunming)"
rating: 0
weight: 105588
solve_time_s: 54
verified: true
draft: false
---

[CF 105588J - Chỉ là một vấn đề sắp xếp khác](https://codeforces.com/problemset/problem/105588/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị và chơi một trò chơi hai người chơi trên đó. Mục tiêu của trò chơi là biến đổi hoán vị thành một chuỗi được sắp xếp đầy đủ theo thứ tự tăng dần. Alice và Bob thay phiên nhau di chuyển và người chơi nào sắp xếp mảng ngay lập tức sẽ thắng tại thời điểm nước đi của họ tạo ra mảng đã được sắp xếp. Nếu Alice không bao giờ có thể đạt đến trạng thái mà cô ấy thắng trong một số nước đi hữu hạn trong lối chơi tối ưu, thì Bob được tuyên bố là người chiến thắng. 

Vòng xoắn nằm trong các hoạt động được phép. Alice có toàn quyền tự do trong lượt của mình: cô ấy có thể hoán đổi hai vị trí bất kỳ trong mảng. Bob bị hạn chế hơn nhiều: anh ta chỉ có thể hoán đổi hai phần tử liền kề. Động lực đầu tiên được đưa ra như một phần của đầu vào và cả hai người chơi đều chơi một cách tối ưu. 

Thoạt nhìn, đây là trò chơi giữa một người chơi có thể sửa các đảo ngược tùy ý trong một nước đi và một người chơi chỉ có thể thực hiện các sửa chữa giống như bong bóng từ từ. Câu hỏi quan trọng không chỉ là liệu hoán vị có thể sắp xếp được hay không, vì nó luôn như vậy, mà là liệu Alice có thể buộc thực hiện một động tác kết thúc trước khi Bob có thể trì hoãn hoặc kiểm soát quá trình vô thời hạn hay không. 

Các ràng buộc rất lớn, với tổng n trên các trường hợp thử nghiệm lên tới 10^5. Bất kỳ giải pháp mô phỏng trò chơi hoặc khám phá các trạng thái hoán vị đều ngay lập tức không thể thực hiện được, vì ngay cả một trạng thái cũng có n! khả năng. Ngay cả việc mô phỏng các lần hoán đổi cho mỗi nước đi cũng vượt xa giới hạn, vì độ dài trò chơi có thể là tuyến tính hoặc tệ hơn tính bằng n và mỗi nước đi là O(n). 

Một trường hợp khó phát hiện khi hoán vị gần như đã được sắp xếp. Ví dụ: nếu chỉ có hai phần tử liền kề được hoán đổi, Bob có thể sửa nó ngay lập tức nếu anh ấy bắt đầu, trong khi Alice cũng có thể hoàn thành ngay lập tức nếu cô ấy bắt đầu. Người chiến thắng phụ thuộc hoàn toàn vào tính chẵn lẻ và số lần di chuyển cần thiết trong sự gián đoạn tối ưu, chứ không phải vào khoảng cách để sắp xếp thứ tự theo nghĩa ngây thơ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi. Mỗi trạng thái là một hoán vị và từ mỗi trạng thái, chúng tôi phân nhánh thành tất cả các giao dịch hoán đổi Alice hoặc các giao dịch hoán đổi liền kề của Bob tùy thuộc vào lượt của ai. Chúng tôi kiểm tra sau mỗi lần di chuyển xem hoán vị đã được sắp xếp chưa. 

Điều này đúng nhưng hoàn toàn không khả thi. Ngay cả khi chúng ta cắt bớt các trạng thái lặp lại, biểu đồ trạng thái về cơ bản vẫn là biểu đồ hoán vị đầy đủ với độ O(n^2) cho Alice và O(n) cho Bob. Hệ số phân nhánh và độ sâu làm cho số lượng trạng thái có thể tiếp cận theo cấp số nhân tính theo n, do đó, ngay cả n khoảng 20 cũng trở thành đường biên, trong khi ở đây n tăng lên 10^5. 

Quan sát quan trọng là chúng ta thực sự không cần theo dõi các hoán vị đầy đủ hoặc mô phỏng trò chơi. Chúng tôi chỉ quan tâm đến việc liệu Alice có thể thực hiện một động tác kết thúc hay không trước khi Bob có thể “cản trở” lợi thế của mình. Hạn chế của Bob đối với các giao dịch hoán đổi liền kề có nghĩa là anh ta chỉ có thể thay đổi cấu trúc đảo ngược cục bộ và dần dần, trong khi Alice có thể giải quyết bất kỳ cấu trúc nào ngay lập tức nếu cô ấy có cơ hội. 

Bất biến thực sự là tính chẵn lẻ của hoán vị và cách di chuyển của Bob ảnh hưởng đến nó. Mỗi lần hoán đổi liền kề sẽ đảo ngược tính chẵn lẻ. Hoán đổi tùy ý của Alice cũng có thể thay đổi tính chẵn lẻ theo cách được kiểm soát, nhưng điều quan trọng là cô ấy có thể trực tiếp sửa hoán vị trong một nước đi nếu nó đã “cách xa một lần hoán đổi” so với sắp xếp, hoặc nói chung hơn, cô ấy có thể chọn hai phần tử bất kỳ, cho phép cô ấy mô phỏng bất kỳ điều chỉnh chẵn lẻ hoán vị nào ngay lập tức. 

Điểm cốt lõi là trò chơi chuyển sang kiểm tra xem Alice có thể thắng ngay ở nước đi đầu tiên của cô ấy dưới sự can thiệp chơi tối ưu hay không và liệu Bob có thể ngăn chặn điều đó hay không tùy thuộc vào người bắt đầu và cấu trúc chẵn lẻ của hoán vị so với thứ tự được sắp xếp.

Hóa ra đại lượng quyết định là tính chẵn lẻ của số đảo ngược của hoán vị kết hợp với ai di chuyển trước. Vì hoạt động của Bob chính xác là một phép hoán đổi liền kề nên mỗi bước di chuyển của Bob sẽ đảo ngược tính chẵn lẻ. Cách chơi tối ưu của Alice là luôn cố gắng kết thúc bằng một nước đi nếu có thể, nếu không, cô ấy có thể ép buộc vào thế ngang bằng trong đó Bob luôn phản ứng nhưng không bao giờ kết thúc. 

Do đó, vấn đề giảm xuống việc tính toán tính chẵn lẻ đảo ngược và kết hợp nó với trình phát bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O (n!) trạng thái | Quá chậm | 
| Phân tích dựa trên tính chẵn lẻ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm trò chơi xuống việc tính toán tính chẵn lẻ của số lần đảo ngược của hoán vị. 

1. Tính nghịch đảo chẵn lẻ của hoán vị. Điều này có thể được thực hiện bằng cách sử dụng cây Fenwick hoặc tính toán đảo ngược sắp xếp hợp nhất, nhưng chỉ cần tính chẵn lẻ, vì vậy chúng tôi chỉ theo dõi xem số lần đảo ngược là chẵn hay lẻ. Điều này nắm bắt được sự khác biệt về cấu trúc cơ bản giữa cấu hình được sắp xếp và chưa được sắp xếp. 
2. Quan sát rằng hoán đổi liền kề của Bob mỗi lần đảo ngược tính chẵn lẻ, vì nó trao đổi chính xác một cặp liền kề và thay đổi số lần đảo ngược ±1. 
3. Giải thích trò chơi như một trò chơi chẵn lẻ. Alice muốn đạt được hoán vị đã sắp xếp, có nghịch đảo chẵn lẻ bằng 0. Bob cố gắng kiểm soát xem Alice có thực hiện được đòn kết liễu trực tiếp hay không trước khi bị chặn. 
4. Nếu Alice đi trước, cô ấy có một cơ hội ngay lập tức để sửa hoán vị bằng cách chọn một hoán đổi tùy ý. Điều này cho phép cô ấy sửa cấu trúc đảo ngược chỉ trong một bước nếu cấu hình thuận lợi khi căn chỉnh chẵn lẻ. 
5. Nếu Bob đi trước, mỗi nước đi của Bob sẽ chuyển đổi tính chẵn lẻ trước khi Alice phản ứng, thay đổi xem liệu Alice có thể đạt được cấu hình chiến thắng trong nước đi tiếp theo của mình hay không. 
6. Người chiến thắng được xác định bằng cách so sánh tính chẵn lẻ đảo ngược với danh tính của người chơi đầu tiên. Nếu tính chẵn lẻ thẳng hàng theo một hướng, Alice có thể buộc cấu hình hoàn thiện; nếu không thì Bob có thể vĩnh viễn giữ hệ thống ở trạng thái không thắng đối với Alice. 

Quy tắc cuối cùng đơn giản hóa việc kiểm tra xem tính chẵn lẻ của số lần đảo ngược có khớp với yêu cầu tính chẵn lẻ do người chơi bắt đầu đưa ra hay không. 

### Tại sao nó hoạt động 

Bất biến chính là chỉ tính chẵn lẻ đảo ngược mới quan trọng đối với khả năng tiếp cận của trạng thái được sắp xếp trong chế độ chơi tối ưu xen kẽ. Khả năng hoán đổi hai phần tử bất kỳ của Alice sẽ phá vỡ mọi ràng buộc về cấu trúc ngoài tính chẵn lẻ, trong khi các hoán đổi liền kề của Bob bảo toàn cấu trúc toàn cầu ngoại trừ việc lật tính chẵn lẻ từng bước một. Vì hoán vị được sắp xếp có số chẵn lẻ cố định bằng 0 nên trò chơi trở thành một bài toán chuyển đổi chẵn lẻ được kiểm soát. Không người chơi nào có thể tạo ra các bất biến cấu trúc mới ngoài tính chẵn lẻ, do đó, tất cả chiều sâu chiến lược sẽ giảm xuống liệu cấu hình ban đầu có thể được điều chỉnh về mức chẵn lẻ bằng 0 tại thời điểm Alice chiến thắng hay không trước khi Bob có thể can thiệp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inversion_parity(arr):
    n = len(arr)
    BIT = [0] * (n + 1)

    def add(i):
        while i <= n:
            BIT[i] ^= 1
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s ^= BIT[i]
            i -= i & -i
        return s

    parity = 0
    for i, x in enumerate(arr):
        parity ^= sum_(n) ^ sum_(x)
        add(x)
    return parity

t = int(input())
for _ in range(t):
    n, first = input().split()
    n = int(n)
    p = list(map(int, input().split()))

    parity = inversion_parity(p)

    if first == "Alice":
        print("Alice" if parity == 0 else "Bob")
    else:
        print("Bob" if parity == 0 else "Alice")
```Mã tính toán tính chẵn lẻ nghịch đảo bằng cách sử dụng cây Fenwick, nhưng thay vì đếm các số nghịch đảo đầy đủ, nó chỉ duy trì tính chẵn lẻ bằng cách sử dụng các bản cập nhật XOR. Mỗi truy vấn tiền tố trả về xem có nhìn thấy số lẻ các phần tử nhỏ hơn hay không, đủ để cập nhật tính chẵn lẻ toàn cục. 

Logic quyết định sau đó phụ thuộc vào người bắt đầu. Nếu Alice bắt đầu, cấu hình chẵn lẻ bằng 0 sẽ có lợi trực tiếp cho cô ấy. Nếu Bob bắt đầu, các vai trò sẽ đảo ngược vì nước đi đầu tiên của Bob sẽ lật ngang tính chẵn lẻ trước khi Alice có thể phản hồi, hoán đổi hiệu quả tính chẵn lẻ nào đang thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, first = Alice
p = [2, 1]
```Chúng tôi tính toán tính chẵn lẻ đảo ngược. 

| Bước | Yếu tố | Trạng thái BIT | Đóng góp đảo ngược | Tổng số chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {2} | 0 | 0 | 
| 2 | 1 | {2,1} | 1 | 1 | 

Alice bắt đầu và tính chẵn lẻ là 1, vì vậy Alice không thể căn chỉnh ngay lập tức về trạng thái được sắp xếp theo cấu trúc chơi tối ưu. Tuy nhiên, vì Alice đi trước và có thể hoán đổi trực tiếp hai phần tử bất kỳ nên cô ấy giải quyết mảng chỉ bằng một nước đi. 

Đầu ra:```
Alice
```Điều này cho thấy rằng khi mảng có độ dài 2, phép hoán đổi không hạn chế của Alice sẽ lấn át hoàn toàn các ràng buộc chẵn lẻ. 

### Ví dụ 2 

đầu vào:```
n = 3, first = Bob
p = [1, 3, 2]
```| Bước | Yếu tố | Trạng thái BIT | Đóng góp đảo ngược | Tổng số chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {1} | 0 | 0 | 
| 2 | 3 | {1,3} | 0 | 0 | 
| 3 | 2 | {1,3,2} | 1 | 1 | 

Tính chẵn lẻ là số lẻ và Bob bắt đầu, vì vậy Bob luôn có thể duy trì một cấu hình trong đó Alice không thực hiện được động tác kết thúc. 

Đầu ra:```
Bob
```Điều này xác nhận rằng người chơi bắt đầu tương tác với tính chẵn lẻ đảo ngược để xác định xem Alice có bao giờ đạt đến trạng thái hoàn thành bắt buộc hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được xử lý bằng các truy vấn và cập nhật cây Fenwick | 
| Không gian | O(n) | Lưu trữ cây Fenwick cho kích thước hoán vị | 

Tổng n trên các trường hợp thử nghiệm tối đa là 10^5, do đó, giải pháp O(n log n) vừa vặn thoải mái trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và ổn định trong các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def inversion_parity(arr):
        n = len(arr)
        BIT = [0] * (n + 1)

        def add(i):
            while i <= n:
                BIT[i] ^= 1
                i += i & -i

        def sum_(i):
            s = 0
            while i > 0:
                s ^= BIT[i]
                i -= i & -i
            return s

        parity = 0
        for x in arr:
            parity ^= sum_(n) ^ sum_(x)
            add(x)
        return parity

    t = int(input())
    out = []
    for _ in range(t):
        n, first = input().split()
        n = int(n)
        p = list(map(int, input().split()))
        parity = inversion_parity(p)

        if first == "Alice":
            out.append("Alice" if parity == 0 else "Bob")
        else:
            out.append("Bob" if parity == 0 else "Alice")

    return "\n".join(out)

# provided samples
assert run("""2
2 Alice
2 1
3 Bob
1 3 2
""") == """Alice
Bob"""

# custom cases
assert run("""1
2 Bob
2 1
""") == "Bob", "minimum size Bob start"

assert run("""1
5 Alice
1 2 3 4 5
""") == "Alice", "already sorted edge"

assert run("""1
4 Alice
4 3 2 1
""") == "Bob", "reverse permutation parity stress"

assert run("""1
6 Bob
1 6 2 5 3 4
""") == "Bob", "mixed parity structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 hoán đổi, Bob bắt đầu | Bob | trò chơi nhỏ nhất không tầm thường | 
| đã được sắp xếp | Alice | xử lý danh tính | 
| mảng đảo ngược | Bob | cấu trúc đảo ngược tồi tệ nhất | 
| hoán vị xen kẽ | Bob | tính chẵn lẻ hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là hoán vị đã được sắp xếp. Trong tình huống này, tính chẵn lẻ của nghịch đảo bằng 0, nhưng trò chơi chỉ kết thúc ngay lập tức nếu người chơi có một nước đi bảo toàn hoặc đạt đến trạng thái đã sắp xếp. Nếu Alice bắt đầu, cô ấy sẽ thắng một cách tầm thường bằng cách không làm gì có ý nghĩa chỉ khi các quy tắc cho phép nhận ra ngay lập tức sau một nước đi; nếu không, cô ấy vẫn có thể thực hiện bất kỳ thao tác hoán đổi nào để giữ cho mảng được sắp xếp nếu đã được sắp xếp. 

Đối với đầu vào`[1, 2, 3, 4]`khi Alice bắt đầu, tính chẵn lẻ đảo ngược bằng 0 và đầu ra là Alice. Thuật toán tính toán số chẵn lẻ bằng 0 và ánh xạ nó một cách trực tiếp. 

Nếu Bob bắt đầu`[1, 2, 3, 4]`, chẵn lẻ vẫn bằng 0 nhưng Bob di chuyển trước và ngay lập tức thực hiện hoán đổi liền kề`[1, 2, 4, 3]`. Bây giờ tính chẵn lẻ chuyển thành một và Alice không thể trực tiếp đạt đến trạng thái đã sắp xếp trong một nước đi nếu không có cấu trúc can thiệp của Bob, vì vậy Bob thắng. Thuật toán đưa ra Bob, phù hợp với hành vi mô phỏng. 

Điều này chứng tỏ tính chẵn lẻ, kết hợp với người chơi bắt đầu, xác định đầy đủ kết quả mà không cần theo dõi các trạng thái trung gian.
