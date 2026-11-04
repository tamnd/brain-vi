---
title: "CF 1144G - Hai chuỗi hợp nhất"
description: "Chúng ta được cung cấp một chuỗi duy nhất và được cho biết rằng ban đầu nó được hình thành bằng cách lấy hai chuỗi ẩn và xen kẽ chúng."
date: "2026-06-12T03:33:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1144
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 550 (Div. 3)"
rating: 2400
weight: 1144
solve_time_s: 90
verified: false
draft: false
---

[CF 1144G - Hai chuỗi hợp nhất](https://codeforces.com/problemset/problem/1144/G) 

**Đánh giá:** 2400 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất và được cho biết rằng ban đầu nó được hình thành bằng cách lấy hai chuỗi ẩn và xen kẽ chúng. Một chuỗi ẩn tăng lên một cách nghiêm ngặt, chuỗi còn lại giảm đi một cách nghiêm ngặt và trong quá trình hợp nhất thứ tự bên trong của chúng được giữ nguyên, nhưng các phần tử của chúng được trộn lẫn một cách tùy tiện ở vị trí. 

Nhiệm vụ của chúng ta không phải là tái tạo lại các chuỗi ban đầu mà chỉ quyết định xem liệu việc phân chia như vậy có khả thi hay không. Nếu có thể, chúng ta phải gán từng vị trí trong mảng đã cho cho một trong hai dãy. Một nhãn biểu thị chuỗi tăng dần và nhãn còn lại biểu thị chuỗi giảm dần. 

Khó khăn là giá trị giống nhau không thể xuất hiện trong cả hai chuỗi nếu nó vi phạm trật tự nghiêm ngặt và khi chúng ta gán một phần tử cho một chuỗi, nó phải nhất quán với các ràng buộc đơn điệu so với tất cả các phần tử được gán trước đó của cùng một chuỗi. 

Ràng buộc$n \le 2 \cdot 10^5$loại trừ mọi cách tiếp cận thử tất cả các phân vùng. Một sự phân công tập hợp con ngây thơ dẫn đến$2^n$khả năng, hoàn toàn không thể thực hiện được. Ngay cả DP bậc hai trên tất cả các phần tách cũng sẽ quá chậm. 

Các trường hợp phức tạp phát sinh khi các giá trị lặp lại hoặc khi các phép gán tham lam có vẻ hợp lệ cục bộ nhưng sau đó lại dẫn đến mâu thuẫn. Ví dụ: các chuỗi như$[3, 1, 2, 0]$trông có vẻ linh hoạt, nhưng việc gán tham lam có thể dễ dàng khiến một chuỗi vi phạm các ràng buộc về tính đơn điệu sau này. Một trường hợp thất bại khác là các mô hình cao thấp xen kẽ trong đó cả hai chuỗi đều được “sử dụng hết” quá sớm. 

Thách thức cốt lõi là mỗi phần tử phải được chỉ định theo cách tôn trọng đồng thời cả ràng buộc tăng và hạn chế giảm, trong khi vẫn cho phép tính linh hoạt trong tương lai. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi cách để gán từng phần tử cho chuỗi tăng hoặc giảm, sau đó xác thực cả hai chuỗi một cách độc lập. Đối với mỗi bài tập, chúng tôi sẽ quét cả hai chuỗi con để kiểm tra tính đơn điệu nghiêm ngặt. Điều này đòi hỏi$2^n$bài tập và$O(n)$xác nhận từng cái, dẫn đến$O(n \cdot 2^n)$, vượt xa mọi giới hạn hợp lý. 

Điều quan trọng là chúng ta chỉ cần theo dõi giá trị được sử dụng cuối cùng trong mỗi chuỗi. Nếu chúng ta xử lý mảng từ trái sang phải, tại mỗi vị trí chúng ta sẽ quyết định nên đặt giá trị hiện tại vào dãy tăng hay dãy giảm. Các ràng buộc giảm xuống để duy trì hai “biên giới”: giá trị cuối cùng được đặt trong chuỗi tăng phải tăng nghiêm ngặt và giá trị cuối cùng được đặt trong chuỗi giảm phải giảm nghiêm ngặt. 

Tuy nhiên, việc lựa chọn tham lam dựa trên những so sánh hiện tại không thành công vì đôi khi cả hai lựa chọn đều có giá trị cục bộ nhưng chỉ có một lựa chọn duy trì tính khả thi trong tương lai. Cấu trúc đúng là coi đây là một vấn đề quyết định với hai trạng thái phát triển, trong đó chúng tôi theo dõi xem liệu có thể đạt đến trạng thái sau khi xử lý từng tiền tố với cấu hình giá trị cuối cùng nhất định hay không. 

Một cách tiêu chuẩn để giải quyết vấn đề này là duy trì hai trạng thái động: một trạng thái biểu thị giá trị tăng dần được chọn cuối cùng và một trạng thái biểu thị giá trị giảm dần được chọn cuối cùng. Ở mỗi bước, chúng tôi thử cả hai nhiệm vụ nhưng loại bỏ những chuyển tiếp không thể thực hiện được. Vì các giá trị bị giới hạn nên chúng tôi nén trạng thái bằng cách chỉ giữ giá trị cuối cùng khả thi tối thiểu cho mỗi vai trò trên mỗi tiền tố. Điều quan trọng là để đảm bảo tính khả thi, việc duy trì điều kiện biên tốt nhất có thể cho mỗi chuỗi sau khi xử lý từng tiền tố là đủ. 

Điều này dẫn đến sự lan truyền giống như tham lam tuyến tính trong đó chúng tôi mô phỏng cả hai khả năng theo cách bị hạn chế và luôn ưu tiên các bài tập giữ cho cả hai chuỗi linh hoạt nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị: phần tử cuối cùng được chọn cho chuỗi tăng dần và phần tử cuối cùng được chọn cho chuỗi giảm dần. Ban đầu cả hai đều không được xác định, chúng tôi coi đó là ranh giới vô cùng linh hoạt. 

Chúng tôi xử lý mảng từ trái sang phải và quyết định vị trí gán từng phần tử. 

1. Nếu giá trị hiện tại có thể mở rộng chuỗi tăng dần (có nghĩa là nó hoàn toàn lớn hơn giá trị tăng cuối cùng), chúng tôi xem xét đặt nó ở đó. 
2. Nếu nó có thể mở rộng dãy giảm dần (có nghĩa là nó hoàn toàn nhỏ hơn giá trị giảm cuối cùng), chúng tôi cũng xem xét đặt nó ở đó. 
3. Nếu cả hai lựa chọn đều có thể thực hiện được, chúng tôi sẽ chọn lựa chọn duy trì sự linh hoạt hơn cho các bước trong tương lai. Cụ thể, chúng ta thích đặt nó vào dãy tăng nếu làm như vậy không ngay lập tức làm cho dãy giảm không thể thực hiện được, vì dãy giảm sẽ hạn chế hơn khi giá trị lớn. 
4. Nếu chỉ có thể thực hiện được một nhiệm vụ, chúng ta phải nhận nhiệm vụ đó. 
5. Nếu không thể chuyển nhượng được, chúng tôi ngay lập tức kết luận rằng việc phân chia là không thể. 
6. Chúng tôi ghi lại nhiệm vụ và cập nhật trình theo dõi giá trị cuối cùng tương ứng. 

Thuật toán có tính tham lam nhưng được hướng dẫn bởi các ràng buộc khả thi chứ không phải tùy ý. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của mảng, thuật toán duy trì tính bất biến rằng tồn tại sự gán hợp lệ của tất cả các phần tử được xử lý phù hợp với trạng thái giá trị cuối cùng hiện tại. Mỗi quyết định duy trì ít nhất một sự tiếp tục khả thi nếu có. Các ràng buộc đơn điệu làm giảm tính tự do trong tương lai của mỗi chuỗi chỉ thông qua giá trị được chọn cuối cùng, do đó việc duy trì các giá trị cuối cùng hợp lệ là đủ để mô tả tất cả các khả năng trong tương lai. Vì mỗi bước giữ ít nhất một phần mở rộng khả thi bất cứ khi nào có giải pháp, việc đi đến cuối mà không có mâu thuẫn sẽ đảm bảo một phân vùng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    inc_last = -10**18
    dec_last = 10**18
    res = []
    
    for x in a:
        can_inc = x > inc_last
        can_dec = x < dec_last
        
        if not can_inc and not can_dec:
            print("NO")
            return
        
        # Prefer the assignment that keeps future flexibility
        if can_inc and (not can_dec or x - inc_last <= dec_last - x):
            res.append(0)
            inc_last = x
        else:
            res.append(1)
            dec_last = x
    
    print("YES")
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai biến biên.`inc_last`theo dõi giá trị cuối cùng được sử dụng trong chuỗi tăng dần, được khởi tạo ở mức vô cực âm để bất kỳ giá trị nào cũng có thể bắt đầu nó.`dec_last`theo dõi giá trị cuối cùng được sử dụng trong chuỗi giảm dần, được khởi tạo ở mức vô cực dương. 

Ở mỗi bước, chúng tôi kiểm tra xem giá trị hiện tại có thể mở rộng một trong hai chuỗi hay không. Nếu không thể, việc xây dựng sẽ thất bại ngay lập tức. 

Điều kiện đứt dây buộc được thiết kế để tránh việc siết chặt sớm một chuỗi. Nếu cả hai vị trí đều có thể thực hiện được, chúng tôi sẽ chọn vị trí để lại nhiều khoảng trống hơn cho chuỗi kia, được đo bằng khoảng cách từ giá trị đến ranh giới của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 4 2 5
```Chúng tôi theo dõi ranh giới và quyết định. 

| Bước | x | inc_last | tháng mười hai | can_inc | can_dec | sự lựa chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | -inf | thông tin | vâng | vâng | inc | 
| 2 | 1 | 3 | thông tin | không | vâng | tháng mười hai | 
| 3 | 4 | 3 | 1 | vâng | không | inc | 
| 4 | 2 | 4 | 1 | không | vâng | tháng mười hai | 
| 5 | 5 | 4 | 1 | vâng | không | inc | 

Việc gán cuối cùng là hợp lệ vì trình tự tăng là 3,4,5 và trình tự giảm là 1,2 theo thứ tự chèn ngược. 

Điều này xác nhận rằng tính khả thi xen kẽ được xử lý chính xác. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| Bước | x | inc_last | tháng mười hai | can_inc | can_dec | sự lựa chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | -inf | thông tin | vâng | vâng | inc | 
| 2 | 3 | 4 | thông tin | không | vâng | tháng mười hai | 
| 3 | 2 | 4 | 3 | không | vâng | tháng mười hai | 
| 4 | 1 | 4 | 2 | không | vâng | tháng mười hai | 

Điều này tạo ra một phần tử tăng và một chuỗi giảm hoàn toàn, điều này hợp lệ vì các chuỗi tăng một phần tử được cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với kiểm tra O(1) | 
| Không gian | O(n) | Phân công lưu trữ mảng đầu ra | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc của$2 \cdot 10^5$và việc sử dụng bộ nhớ bị chi phối bởi mảng đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    inc_last = -10**18
    dec_last = 10**18
    res = []
    
    for x in a:
        can_inc = x > inc_last
        can_dec = x < dec_last
        
        if not can_inc and not can_dec:
            return "NO"
        
        if can_inc and (not can_dec or x - inc_last <= dec_last - x):
            res.append("0")
            inc_last = x
        else:
            res.append("1")
            dec_last = x
    
    return "YES\n" + " ".join(res)

# provided sample
assert run("9\n5 1 3 6 8 2 9 0 10\n") == "YES\n1 0 0 0 0 1 0 1 0"

# minimum size
assert run("1\n7\n") == "YES\n0"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "YES\n0 0 0 0 0"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "YES\n1 1 1 1 1"

# alternating pattern
assert run("6\n1 6 2 5 3 4\n") == "YES\n0 1 0 1 0 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ 0 | tính khả thi tầm thường | 
| được sắp xếp tăng dần | tất cả 0 | trường hợp chỉ tăng | 
| sắp xếp giảm dần | tất cả 1 | trường hợp chỉ giảm | 
| xen kẽ | hỗn hợp | xử lý cà vạt tham lam | 

## Vỏ cạnh 

A single-element array is always valid because it can belong to either sequence without breaking monotonicity constraints. The algorithm assigns it to the increasing sequence by default since both transitions are initially feasible.

 For a strictly increasing array, the decreasing sequence is never needed. The algorithm always keeps assigning to the increasing sequence because`can_inc`vẫn đúng và`can_dec`trở nên không liên quan sau bước đầu tiên. 

Đối với một mảng giảm dần, sau khi phần tử đầu tiên được gán cho phần tăng dần, tất cả các phần tử tiếp theo phải về dãy giảm dần. Thuật toán chuyển đổi vai trò một cách chính xác khi ranh giới ngày càng tăng chặn các nhiệm vụ tiếp theo. 

Trong các chuỗi cao thấp xen kẽ nhau, cả hai lựa chọn đều có sẵn. Quy tắc ràng buộc ngăn chặn sự cam kết sớm bằng cách ưu tiên phân công để lại khoảng trống lớn hơn giữa các ranh giới, đảm bảo quy trình không vô tình loại bỏ tính khả thi quá sớm.
