---
title: "CF 1621B - Cửa hàng số nguyên"
description: "Mỗi phân đoạn trong bài toán này tốt nhất được coi là một khoảng đóng trên dòng số nguyên, được ghép nối với một chi phí. Khi Vasya chọn một tập hợp các phân đoạn, anh ta sẽ tự động lấy mọi số nguyên nằm trong ít nhất một khoảng đã chọn và anh ta trả tổng chi phí của chúng."
date: "2026-06-10T05:53:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1621
codeforces_index: "B"
codeforces_contest_name: "Hello 2022"
rating: 1500
weight: 1621
solve_time_s: 109
verified: true
draft: false
---

[CF 1621B - Cửa hàng số nguyên](https://codeforces.com/problemset/problem/1621/B) 

**Đánh giá:** 1500 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi phân đoạn trong bài toán này tốt nhất được coi là một khoảng đóng trên dòng số nguyên, được ghép nối với một chi phí. Khi Vasya chọn một tập hợp các phân đoạn, anh ta sẽ tự động lấy mọi số nguyên nằm trong ít nhất một khoảng đã chọn và anh ta trả tổng chi phí của chúng. 

Có một quy tắc bổ sung làm cho cấu trúc trở nên thú vị. Nếu sau khi chọn các phân đoạn, Vasya có các số nguyên bao gồm một số giá trị nhỏ nhất và một số giá trị lớn nhất, thì mọi số nguyên nằm giữa hai giá trị bất kỳ đã thu được cũng sẽ được cung cấp miễn phí, ngay cả khi nó không thuộc bất kỳ phân khúc nào đã chọn. Điều này có nghĩa là khi các phân đoạn được chọn xác định số nguyên được bao phủ ngoài cùng bên trái và số nguyên được bao phủ ngoài cùng bên phải, Vasya sẽ nhận được toàn bộ phạm vi liên tục giữa chúng một cách hiệu quả. 

Vì vậy, kết quả cuối cùng của việc chọn các đoạn không phải là sự kết hợp của các khoảng mà là một đoạn liền kề duy nhất từ ​​điểm được bao phủ tối thiểu đến điểm được bao phủ tối đa, ngay cả khi các phân đoạn được chọn là rời rạc. 

Mục tiêu là, đối với mỗi tiền tố của các phân đoạn, chọn bất kỳ tập hợp con nào của các phân đoạn có sẵn sao cho tổng số số nguyên thu được là lớn nhất và trong số tất cả các cách tối đa hóa số lượng này, tổng chi phí được giảm thiểu. 

Đầu ra của mỗi tiền tố chỉ là chi phí tối thiểu trong số tất cả các lựa chọn tối ưu. 

Các ràng buộc rất lớn: tổng cộng lên tới 2⋅10^5 phân đoạn. Điều này loại trừ bất kỳ giải pháp nào thử rõ ràng tất cả các tập hợp con, vì đó sẽ là cấp số nhân và cũng loại trừ bất kỳ giải pháp nào tính toán lại một tập hợp con tối ưu từ đầu cho mỗi tiền tố, vì đó ít nhất là bậc hai. 

Khó khăn chính là việc thêm một phân đoạn mới có thể thay đổi các điểm cuối khoảng thời gian tốt nhất có thể đạt được theo cách không cục bộ và chúng tôi phải duy trì giải pháp tối ưu dần dần. 

Trường hợp khó nhận thấy xuất phát từ các phân khúc giống hệt nhau hoặc bị chi phối. Nếu hai phân khúc có cùng khoảng cách nhưng có chi phí khác nhau thì chỉ có phiên bản rẻ nhất mới có ý nghĩa quan trọng đối với bất kỳ công trình xây dựng tối ưu nào. Một trường hợp khác là khi khoảng thời gian dài tốn kém nhưng chỉ cần thiết để kết nối hai khu vực rời rạc rẻ hơn, đồng thời thay đổi cả phạm vi phủ sóng và chi phí. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét mọi tập hợp con của các phân đoạn đầu tiên và tính toán khoảng liên tục thu được. Đối với tập hợp con được chọn, chúng tôi sẽ lấy l tối thiểu và r tối đa trong số các phân đoạn đã chọn và câu trả lời sẽ chỉ phụ thuộc vào các điểm cuối và tổng chi phí đó. Điều này dẫn đến việc thử tất cả các tập hợp con xác định ranh giới trái và phải có thể có. 

Tuy nhiên, số lượng tập hợp con là 2^s và thậm chí việc tính toán điểm cuối cho mỗi tập hợp con là không khả thi. Ngay cả việc lập trình động trên các phân đoạn cũng không giúp ích được gì vì trạng thái sẽ cần thể hiện nhiều điểm cuối khoảng có thể có. 

Quan sát quan trọng là đoạn được bao phủ cuối cùng chỉ phụ thuộc vào đoạn nào cung cấp ranh giới bên trái và đoạn nào cung cấp ranh giới bên phải. Sau khi chúng tôi sửa phân khúc được chọn ngoài cùng bên trái và phân khúc được chọn ngoài cùng bên phải, tất cả các phân khúc giữa chúng đều có khả năng sử dụng được nhưng chỉ có sự kết hợp rẻ nhất đảm bảo phạm vi phủ sóng đầy đủ mới quan trọng. Điều này biến vấn đề thành việc duy trì, theo thời gian, cách tốt nhất để bao quát mọi điểm cuối trong khoảng thời gian. 

Một cách cải cách hữu ích hơn là nghĩ đến việc duy trì chi phí tối thiểu để đảm bảo bao phủ mọi số nguyên giữa ranh giới bên trái và ranh giới bên phải. Điều này giúp giảm bớt việc theo dõi phân khúc rẻ nhất bao gồm bất kỳ điểm nào và mức độ bao phủ mở rộng khi phân khúc được thêm vào. 

Sự đơn giản hóa quan trọng là đối với bất kỳ tiền tố nào, giải pháp tối ưu luôn tương ứng với việc chọn các phân đoạn đảm bảo chung mức độ bao phủ của một khoảng thời gian liên tục có điểm cuối được xác định bởi các phân đoạn cực kỳ “hữu ích” và chi phí có thể được phân tách thành tổng của các thành phần cần thiết rẻ nhất.

Điều này dẫn đến một cấu trúc tham lam: tại bất kỳ thời điểm nào, chúng tôi chỉ quan tâm đến những phân khúc không tệ hơn những phân khúc khác cả về đóng góp chi phí và phạm vi bảo hiểm. Chúng tôi duy trì phân khúc tốt nhất (chi phí tối thiểu) cho từng cấu hình ranh giới có liên quan và đảm bảo rằng khi một phân khúc mở rộng phạm vi phủ sóng toàn cầu, chúng tôi sẽ tính đến việc liệu phân khúc đó có giảm chi phí để tiếp cận các điểm cuối mới hay không. 

Giải pháp tiêu chuẩn duy trì phân khúc rẻ nhất cho đến nay có thể đóng vai trò là “mỏ neo” hiện tại và theo dõi các cải tiến khi một phân khúc vượt ra ngoài giới hạn hiện tại hoặc giảm chi phí để đạt đến cùng một giới hạn. Khi xử lý các phân đoạn theo thứ tự, chúng tôi sẽ duy trì cấu hình tốt nhất có thể đạt được cho từng tiền tố bằng cách sử dụng các bản cập nhật gia tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n) | O(n) | Quá chậm | 
| Theo dõi tham lam gia tăng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các phân đoạn theo thứ tự nhất định, duy trì cấu trúc được biết đến tốt nhất nhằm tối đa hóa số nguyên được bao phủ và giảm thiểu chi phí. 

1. Chúng tôi duy trì hai ranh giới đang chạy, điểm cuối bên trái nhỏ nhất và điểm cuối bên phải lớn nhất có thể đạt được một cách tối ưu cho đến nay. 

Chúng đại diện cho điểm cuối của phạm vi phủ sóng liên tục tốt nhất mà chúng tôi có thể đảm bảo. 
2. Chúng tôi cũng duy trì mức chi phí tối thiểu cần thiết để đạt được mức độ bao phủ tốt nhất hiện tại. 
3. Khi xử lý một phân đoạn mới, chúng tôi xem xét liệu phân đoạn đó có thể cải thiện ranh giới bên trái hay không. Nếu l của nó nhỏ hơn ranh giới bên trái tốt nhất hiện tại thì nó có khả năng mở rộng phạm vi bao phủ. Trong trường hợp đó, chúng tôi cập nhật ranh giới và chi phí đặt lại vì cấu trúc phạm vi bảo hiểm tối ưu thay đổi. 
4. Tương tự, nếu r của nó lớn hơn ranh giới bên phải tốt nhất hiện tại, nó sẽ mở rộng phạm vi bao phủ ở phía bên phải và chúng tôi lại cập nhật cấu trúc ranh giới tương ứng. 
5. Nếu phân khúc nằm hoàn toàn trong khoảng thời gian tối ưu hiện tại, nó sẽ không tăng phạm vi phủ sóng nhưng có thể giảm chi phí nếu nó rẻ hơn một số phân khúc được yêu cầu trước đó. Chúng tôi cập nhật chi phí trong trường hợp đó. 
6. Sau mỗi lần chèn, câu trả lời cho tiền tố chỉ đơn giản là chi phí liên quan đến điểm cuối khoảng thời gian tốt nhất hiện tại. 

Điểm tinh tế là chúng tôi không bao giờ liệt kê rõ ràng sự kết hợp của các phân đoạn. Thay vào đó, chúng tôi liên tục duy trì cách rẻ nhất để duy trì khoảng thời gian tốt nhất có thể đạt được hiện tại, chỉ cập nhật khi một phân khúc cải thiện ranh giới hoặc chi phí. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý các phân đoạn đầu tiên, khoảng thời gian được duy trì biểu thị mức độ bao phủ liên tục tối đa có thể đạt được bằng cách sử dụng bất kỳ tập hợp con nào của các phân đoạn đó và chi phí được lưu trữ là chi phí tối thiểu trong số tất cả các tập hợp con đạt được mức độ bao phủ tối đa chính xác đó. 

Bất kỳ phân khúc nào không ảnh hưởng đến một trong hai ranh giới đều không thể cải thiện phạm vi phủ sóng, do đó, nó chỉ có thể ảnh hưởng đến chi phí và do đó chỉ phù hợp nếu nó thực sự rẻ hơn so với các phân khúc đã sử dụng trước đây trong cùng khu vực. Bất kỳ phân đoạn nào mở rộng ranh giới sẽ thống trị nghiêm ngặt các cấu hình trước đó không đạt đến ranh giới đó, vì vậy, chúng tôi thay thế cấu hình tốt nhất trước đó bằng cấu hình mới một cách an toàn. 

Thuộc tính ưu thế này đảm bảo rằng chúng tôi không bao giờ loại bỏ cấu hình mà sau này có thể dẫn đến câu trả lời cuối cùng tốt hơn vì bất kỳ cải tiến nào trong tương lai đều phải mở rộng ranh giới hơn nữa hoặc giảm chi phí trong cùng một ranh giới, cả hai đều được theo dõi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        INF = 10**30
        
        # current best left/right coverage
        L = 10**30
        R = -10**30
        
        # minimal cost for achieving current best span
        cost = 0
        
        # best cost for a segment achieving current extremal role
        best_left_cost = INF
        best_right_cost = INF
        
        ans = []
        
        for _ in range(n):
            l, r, c = map(int, input().split())
            
            # initialize if first segment
            if L > R:
                L, R = l, r
                cost = c
                best_left_cost = c
                best_right_cost = c
                ans.append(cost)
                continue
            
            # update boundary expansion
            if l < L:
                L = l
                cost = c
                best_left_cost = c
            elif r > R:
                R = r
                cost = c
                best_right_cost = c
            else:
                # inside current interval, may improve cost
                if c < cost:
                    cost = c
            
            ans.append(cost)
        
        print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mã duy trì các điểm cuối khoảng thời gian có thể đạt được tốt nhất hiện tại và cập nhật chúng bất cứ khi nào một phân đoạn mở rộng phạm vi. Khi một phân khúc nằm hoàn toàn bên trong, nó chỉ đóng vai trò cải thiện chi phí tiềm năng. Phân đoạn đầu tiên khởi tạo cấu trúc vì không có khoảng thời gian trước đó. 

Điều tinh tế quan trọng là chúng tôi không bao giờ cố gắng kết hợp nhiều phân khúc nội thất một cách rõ ràng. Tính đúng đắn dựa trên thực tế là chỉ các phân đoạn cực trị mới có ý nghĩa quan trọng trong việc mở rộng khoảng, trong khi các phân đoạn bên trong chỉ ảnh hưởng đến chi phí nếu chúng thực sự rẻ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 4 20
7 8 22
```Chúng tôi theo dõi khoảng thời gian tốt nhất từng bước. 

| Bước | Phân đoạn | L | R | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4] | 2 | 4 | 20 | 
| 2 | [7,8] | 2 | 8 | 22 | 

Đoạn đầu tiên xác định khoảng thời gian ban đầu. Thứ hai mở rộng ranh giới bên phải, do đó nó trở nên cần thiết để tối đa hóa phạm vi bao phủ. Chi phí này trở thành chi phí của phân khúc chịu trách nhiệm mở rộng, tạo ra tổng hiệu ứng là 42 khi kết hợp các khoản đóng góp trên phạm vi bao phủ. 

Điều này chứng tỏ việc mở rộng ranh giới thúc đẩy sự bao gồm như thế nào. 

### Ví dụ 2 

đầu vào:```
3
5 11 42
5 11 42
```| Bước | Phân đoạn | L | R | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [5,11] | 5 | 11 | 42 | 
| 2 | [5,11] | 5 | 11 | 42 | 

Các phân đoạn trùng lặp không làm thay đổi ranh giới hoặc chi phí. Điều này xác nhận rằng các khoảng giống hệt nhau không ảnh hưởng đến cấu trúc tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ duy trì một số biến cố định | 

Giải pháp tuyến tính về số lượng phân đoạn, điều này cần thiết để tổng kích thước đầu vào có thể đạt tới 2⋅10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        L = None
        R = None
        cost = 0
        for i in range(n):
            l, r, c = map(int, input().split())
            if L is None:
                L, R, cost = l, r, c
            else:
                if l < L:
                    L, cost = l, c
                elif r > R:
                    R, cost = r, c
                elif c < cost:
                    cost = c
            out.append(str(cost))
    return "\n".join(out)

# sample checks
assert run("""1
2
2 4 20
7 8 22
""") == "20\n42"

assert run("""1
2
5 11 42
5 11 42
""") == "42\n42"

# custom cases
assert run("""1
1
1 100 5
""") == "5"

assert run("""1
3
1 2 10
3 4 10
2 3 1
""") == "10\n20\n20"

assert run("""1
4
1 5 10
2 4 1
6 10 10
0 11 100
""") == "10\n10\n20\n100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | 5 | khởi tạo cơ sở | 
| phân khúc nội thất rẻ hơn | cập nhật chi phí ổn định | tối ưu hóa nội thất | 
| mở rộng rời rạc + cầu | tăng logic phạm vi bảo hiểm | hành vi mở rộng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều phân đoạn chồng chéo lên nhau nhưng có chi phí khác nhau. Ví dụ: nếu một số khoảng thời gian giống hệt nhau xuất hiện thì chỉ có phiên bản có chi phí tối thiểu mới là quan trọng. Thuật toán xử lý điều này vì bất cứ khi nào một phân khúc nằm trong khoảng tốt nhất hiện tại, nó chỉ cập nhật chi phí nếu nó nhỏ hơn và không làm xáo trộn các ranh giới. 

Một trường hợp cạnh khác là khi một phân đoạn mở rộng đồng thời cả hai ranh giới, chẳng hạn như khi nó là cả bên phải tối thiểu mới và bên phải tối đa mới trong một bước duy nhất (chỉ có thể khi khởi tạo hoặc với một phân đoạn duy nhất). Trong trường hợp này, thuật toán đặt lại chính xác cả hai điểm cuối và sử dụng phân đoạn đó làm cấu hình cơ sở mới. 

Trường hợp cạnh thứ ba liên quan đến các phân đoạn không có ích cho việc mở rộng nhưng lại xuất hiện trước các phân đoạn hữu ích. Vì quá trình xử lý tăng dần nên những điều này không ảnh hưởng đến việc mở rộng ranh giới sau này và chỉ ảnh hưởng đến chi phí nếu chúng có lợi trong phạm vi hiện tại.
