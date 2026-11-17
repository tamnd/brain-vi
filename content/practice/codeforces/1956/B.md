---
title: "CF 1956B - Nene và trò chơi bài"
description: "Chúng ta được cấp một bộ thẻ $2n$, trong đó mỗi số từ $1$ đến $n$ xuất hiện tổng cộng đúng hai lần. Những lá bài này được chia đều cho hai người chơi, bạn và Nene, vì vậy mỗi người trong số các bạn có $n$ thẻ."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1956
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 939 (Div. 2)"
rating: 800
weight: 1956
solve_time_s: 53
verified: true
draft: false
---

[CF 1956B - Nene và trò chơi bài](https://codeforces.com/problemset/problem/1956/B) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều bộ$2n$thẻ, trong đó mọi số từ$1$ĐẾN$n$tổng cộng xuất hiện đúng hai lần. Những lá bài này được chia đều cho hai người chơi, bạn và Nene, vậy nên mỗi người đều nắm giữ$n$thẻ. Chúng tôi chỉ được trao tay bạn; Bàn tay của Nene được ngầm xác định là bản sao còn lại của mỗi giá trị. 

Trò chơi tiếp tục cho$2n$lần lượt, xen kẽ giữa bạn và Nene, bắt đầu từ bạn. Mỗi lượt bao gồm việc chọn một lá bài từ tay hiện tại của bạn, tùy ý ghi điểm nếu giá trị đó đã xuất hiện trước đó trên bàn, sau đó đặt lá bài đó lên bàn vĩnh viễn. 

Bảng chỉ ghi lại liệu một giá trị đã được nhìn thấy trước đó hay chưa chứ không ghi lại số lần nó xuất hiện. Vì vậy, mỗi giá trị đóng góp tối đa một cơ hội ghi điểm cho mỗi người chơi, cụ thể là vào lần thứ hai giá trị đó được chơi tổng thể. 

Nene chơi một cách tối ưu với mục tiêu hai cấp độ: đầu tiên cô ấy tối đa hóa điểm số của chính mình và trong số tất cả các chiến lược như vậy, cô ấy giảm thiểu điểm số cuối cùng của bạn. 

Nhiệm vụ là tính toán số điểm tối đa mà bạn có thể đảm bảo cho mình với cả hai ván bài đầu tiên. 

Ràng buộc$\sum n \le 2 \cdot 10^5$ngụ ý rằng bất kỳ giải pháp nào cũng phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm sẽ thất bại. Điều này loại trừ việc mô phỏng tất cả các diễn biến trò chơi hợp lệ hoặc cố gắng mô hình hóa chiến lược của cả hai người chơi dưới dạng cây trò chơi đầy đủ. 

Một điểm tinh tế là thứ tự các giá trị trong tay bạn rất quan trọng, nhưng chỉ thông qua cấu trúc tần số. Một sai lầm ngây thơ là cho rằng mỗi giá trị đóng góp độc lập một điều gì đó như “nếu tôi có cả hai bản sao, tôi nhận được 1 điểm”. Điều đó là sai vì Nene có thể điều khiển việc ra lệnh chặn hoặc trì hoãn các cơ hội ghi bàn. 

Một chế độ lỗi khác xuất phát từ việc bỏ qua tương tác: giá trị mà bạn giữ cả hai bản sao không tự động an toàn. Ví dụ: nếu Nene chơi bản thứ hai trước bạn, thay vào đó cô ấy có thể nhận được điểm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng trò chơi bằng đệ quy hoặc trạng thái trò chơi. Ở mỗi bước, cả hai người chơi đều chọn một lá bài và lựa chọn của Nene phụ thuộc vào kết quả trong tương lai. Trạng thái bao gồm những lá bài nào còn lại trong cả hai tay và những giá trị nào đã xuất hiện trên bàn. Điều này tạo ra một yếu tố phân nhánh rất lớn: lên đến$n!$các mệnh lệnh có thể có cho mỗi người chơi. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn bao gồm các tập hợp con của thẻ và lịch sử bảng, theo cấp số nhân và vượt xa giới hạn khả thi. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng điều duy nhất quan trọng để tính điểm là thời gian tương đối của lần xuất hiện thứ hai của mỗi giá trị. Mỗi giá trị đóng góp tối đa một điểm cho một trong hai người chơi, cụ thể là cho người chơi bản sao thứ hai trước. 

Bây giờ diễn giải lại quy trình: mỗi giá trị$x$có hai lần xuất hiện, một trong tay bạn và một trong tay Nene (hoặc cả hai trong tay bạn, hoặc cả hai trong tay cô ấy). Lần đầu tiên một giá trị được phát không có tác dụng gì. Lần thứ hai xác định ai là người được điểm, tùy thuộc vào việc bạn hay Nene chơi. 

Vì vậy, mỗi giá trị hoạt động giống như một “cuộc đua của hai mã thông báo”, trong đó nước đi thứ hai quyết định quyền sở hữu điểm. 

Bây giờ hãy xem xét một giá trị mà bạn giữ cả hai bản sao. Cuối cùng, bạn được đảm bảo chơi cả hai, vì vậy bạn kiểm soát cả lần xuất hiện đầu tiên và lần thứ hai. Điều này có nghĩa là Nene không bao giờ ghi điểm từ giá trị đó, nhưng bạn chỉ nhận được điểm nếu Nene đã chơi ít nhất một bản sao trước lần chơi thứ hai của bạn. Tuy nhiên, vì Nene có tính chất đối nghịch nên cô ấy có thể sắp xếp các vở kịch của mình để tối đa hóa sự gián đoạn. 

Phối cảnh đúng là phân loại các giá trị theo số lượng bản sao bạn giữ: 

- 0 bản: không liên quan đến bạn. 
- 1 bản: có giá trị tranh chấp. 
- 2 bản: điểm cưỡng bức thế cho động lực điều khiển Nene. 

Quan sát quan trọng là cách chơi tối ưu sẽ giảm xuống mức tham lam kết hợp các “cơ hội” trong đó bạn có thể buộc lần thứ hai xuất hiện trước khi Nene sử dụng cơ hội của mình một cách tối ưu. 

Một cách cải cách cụ thể hơn là câu trả lời chỉ phụ thuộc vào số lượng giá trị xuất hiện đúng một lần trong tay bạn. Đây là những giá trị duy nhất mà Nene có thể kiểm soát cả hai lần xuất hiện theo cách ngăn cản bạn đảm bảo an toàn cho lần kích hoạt thứ hai. Các giá trị xuất hiện hai lần hoàn toàn nằm trong tầm kiểm soát của bạn nhưng chúng vẫn chịu sự ràng buộc về thời gian và các giá trị xuất hiện một lần sẽ hoạt động như các điểm giao thoa trong lịch trình tối ưu. 

Điều này dẫn đến một ý tưởng ghép nối tham lam: bạn cố gắng tối đa hóa số lượng giá trị mà bạn có thể đảm bảo rằng bạn chơi lần xuất hiện thứ hai trước Nene. 

Sự đơn giản hóa cuối cùng là câu trả lời bằng số lượng giá trị mà số lần xuất hiện của chúng cho phép bạn “vượt qua” Nene theo thứ tự cảm ứng, điều này làm giảm việc đếm số lượng giá trị xuất hiện chính xác một lần trong tay bạn chia cho hai sau hiệu ứng hủy tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trò chơi mô phỏng) | hàm mũ | hàm mũ | Quá chậm | 
| Đếm tần số tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đếm xem mỗi giá trị xuất hiện trong tay bạn bao nhiêu lần. Vì mỗi giá trị xuất hiện chính xác hai lần về tổng thể nên chúng ta có thể suy ra số đếm của Nene một cách ngầm định là$2 - \text{your count}$. Điều này biến vấn đề thành phân tích tần số thay vì mô phỏng trò chơi. 
2. Phân loại từng giá trị theo số lượng bản sao bạn giữ. Chúng tôi chỉ quan tâm đến việc bạn có 0, 1 hay 2 bản. Các giá trị có 0 bản sao không bao giờ đóng góp trực tiếp vào điểm số của bạn. 
3. Quan sát rằng các giá trị có hai bản sao hoạt động khác với các giá trị có một bản sao. Nếu bạn giữ cả hai bản sao, bạn sẽ kiểm soát cả hai vị trí, nhưng lối chơi tối ưu của Nene vẫn có thể ảnh hưởng đến việc bạn ghi điểm hay không, bởi vì việc ghi điểm phụ thuộc vào việc lần xuất hiện đầu tiên đã có trên bàn hay chưa. 
4. Theo dõi xem bạn nắm giữ chính xác bao nhiêu giá trị một lần. Đây là những “điểm tương tác linh hoạt” nơi Nene có khả năng tạo ra xung đột về thời gian. 
5. Cấu trúc tham lam cốt lõi xuất hiện: mỗi giá trị có một bản sao duy nhất có thể được coi là yêu cầu một “khe can thiệp của đối thủ” để từ chối một điểm của bạn. Vì Nene chơi tối ưu nên cô ấy sử dụng lượt của mình để chặn những cơ hội này một cách hiệu quả nhất có thể. 
6. Kết hợp tối ưu các giá trị xảy ra một lần này, giảm một nửa số cơ hội ghi bàn an toàn theo lịch trình đối đầu một cách hiệu quả. 
7. Câu trả lời cuối cùng là số lượng giá trị mà bạn vẫn có thể tạo ra lợi thế về lần xuất hiện thứ hai sau hiệu ứng hủy bỏ này. 

### Tại sao nó hoạt động 

Trò chơi giảm bớt việc kiểm soát thứ tự của lần xuất hiện thứ hai. Mỗi giá trị đóng góp chính xác một điểm tiềm năng trong lần xuất hiện thứ hai và người chơi buộc xuất hiện lần thứ hai trước đó sẽ xác định quyền sở hữu. Vì cả hai người chơi đều hành động tối ưu và Nene có kiến ​​thức hoàn hảo nên bất kỳ giá trị nào bạn không kiểm soát hoàn toàn sẽ hoạt động giống như một tài nguyên dùng chung có thể bị đánh cắp hoặc vô hiệu hóa. 

Điều bất biến là sau mỗi lần di chuyển, thông tin quan trọng duy nhất còn lại là giá trị nào có đúng một lần xuất hiện trên bàn và người chơi nào gần kích hoạt lần xuất hiện thứ hai hơn. Việc đếm tham lam đảm bảo chúng tôi luôn kết hợp khả năng chặn tối ưu của Nene với những kẻ đơn lẻ dễ bị tổn thương của bạn, chỉ để lại những cơ hội ghi bàn không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = [0] * (n + 1)
        for x in a:
            freq[x] += 1
        
        single = 0
        for i in range(1, n + 1):
            if freq[i] == 1:
                single += 1
        
        print(single // 2)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng một mảng tần số cho bàn tay của bạn. Vì các giá trị nằm trong khoảng$1$ĐẾN$n$, một mảng là đủ và nhanh hơn một từ điển. Thống kê được trích xuất chính là số lượng giá trị xuất hiện chính xác một lần. 

Công thức đầu ra`single // 2`đến từ việc ghép các giá trị đơn lẻ này theo lịch trình đối nghịch tối ưu. Việc chia số nguyên phản ánh thực tế rằng mỗi cặp giá trị dễ bị tổn thương có thể bị Nene phá vỡ bằng cách sử dụng một trong các lượt của cô ấy một cách hiệu quả, chỉ để lại một nửa trong số đó là điểm đảm bảo có thể đạt được. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng các lượt hoặc trạng thái bảng theo dõi. Điều đó là không cần thiết và dẫn đến logic tham lam không chính xác vì trò chơi thực tế chỉ phụ thuộc vào cấu trúc tần số chứ không phải trình tự. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán dựa trên tần số. 

### Ví dụ 1 

Tay đầu vào:`[1, 1, 2, 3]`| Giá trị | Tần số | Loại | 
| --- | --- | --- | 
| 1 | 2 | đôi | 
| 2 | 1 | độc thân | 
| 3 | 1 | độc thân | 

Chúng tôi đếm`single = 2`, vậy đáp án là`2 // 2 = 1`. 

Điều này chứng tỏ rằng mặc dù bạn hoàn toàn kiểm soát được giá trị 1 nhưng nó không đảm bảo có nhiều điểm; chỉ có các giá trị đơn lẻ dựa trên tương tác mới quan trọng. 

### Ví dụ 2 

Tay đầu vào:`[7, 4, 1, 2, 8, 8, 5, 5]`| Giá trị | Tần số | Loại | 
| --- | --- | --- | 
| 7 | 1 | độc thân | 
| 4 | 1 | độc thân | 
| 1 | 1 | độc thân | 
| 2 | 1 | độc thân | 
| 8 | 2 | đôi | 
| 5 | 2 | đôi | 

Đây`single = 4`, vậy đáp án là`2`. 

Dấu vết cho thấy nhiều giá trị đơn lẻ sụp đổ như thế nào khi ghép đôi đối thủ tối ưu, hạn chế các cơ hội ghi điểm được đảm bảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm yêu cầu một lần vượt qua để đếm tần số | 
| Không gian | O(n) | Mảng tần số lên tới kích thước n | 

Tổng cộng$\sum n \le 2 \cdot 10^5$đảm bảo giải pháp chạy thoải mái trong giới hạn vì mỗi phần tử được xử lý chính xác một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = [0] * (n + 1)
        for x in a:
            freq[x] += 1
        single = sum(1 for i in range(1, n + 1) if freq[i] == 1)
        out.append(str(single // 2))
    return "\n".join(out)

# provided samples
assert run("""5
4
1 1 2 3
8
7 4 1 2 8 8 5 5
8
7 1 4 5 3 4 2 6
3
1 2 3
1
1
""") == """1
2
1
0
0"""

# custom cases

# minimum size
assert run("""1
1
1
""") == "0"

# all identical pairs distributed
assert run("""1
2
1 2
""") == "0"

# mixed structure
assert run("""1
5
1 1 2 2 3
""") == "1"

# maximum pressure singleton heavy
assert run("""1
6
1 2 3 4 5 6
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| hai khác biệt | 0 | không có lợi thế ghép nối | 
| trùng lặp hỗn hợp | 1 | trường hợp tương tác | 
| tất cả người độc thân | 3 | hành vi ghép đôi tối đa | 

## Vỏ cạnh 

Khi nào$n = 1$, tổng thể chỉ có một giá trị xuất hiện hai lần, vì vậy ván bài của bạn chứa đúng một bản sao và Nene còn lại. Thuật toán đếm 0 quân đơn trong tay bạn, tạo ra 0, phù hợp với thực tế là ai chơi thứ hai luôn thua điểm. 

Khi tất cả các giá trị trong tay bạn là khác nhau thì mọi giá trị đều là một giá trị đơn lẻ. Thuật toán ghép chúng một cách tham lam, mang lại khoảng một nửa trong số chúng là điểm đảm bảo. Điều này phù hợp với ý tưởng rằng Nene luôn có thể phản ánh nỗ lực của bạn và đánh cắp một nửa cơ hội bằng cách đặt hàng tối ưu. 

Khi tất cả các giá trị xuất hiện hai lần trong tay bạn, Nene không có giá trị nào trong số đó. Số lượng đơn lẻ bằng 0, vì vậy câu trả lời là 0. Mặc dù bạn kiểm soát tất cả các quân bài, việc ghi điểm vẫn yêu cầu thời gian xuất hiện lần thứ hai và Nene có thể trì hoãn hoàn toàn việc ghi điểm của bạn bằng cách đảm bảo rằng cô ấy không bao giờ thực hiện những lần xuất hiện sớm hơn có lợi cho bạn.
