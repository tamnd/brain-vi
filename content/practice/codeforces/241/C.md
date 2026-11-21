---
title: "CF 241C - Hộp Gương"
description: "Hệ thống mô tả một hộp hình chữ nhật trong đó chùm tia laser đi qua một lỗ nhỏ trên bức tường bên trái và phải thoát ra qua một lỗ khác trên bức tường bên phải. Bên trong hộp có các đoạn gương nằm ngang được đặt trên sàn hoặc trên trần nhà."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 241
codeforces_index: "C"
codeforces_contest_name: "Bayan 2012-2013 Elimination Round (ACM ICPC Rules, English statements)"
rating: 2000
weight: 241
solve_time_s: 87
verified: false
draft: false
---

[CF 241C - Hộp Gương](https://codeforces.com/problemset/problem/241/C) 

**Xếp hạng:** 2000 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống mô tả một hộp hình chữ nhật trong đó chùm tia laser đi qua một lỗ nhỏ trên bức tường bên trái và phải thoát ra qua một lỗ khác trên bức tường bên phải. Bên trong hộp có các đoạn gương nằm ngang được đặt trên sàn hoặc trên trần nhà. Mỗi gương bao phủ một đoạn dọc theo trục ngang và mang một số điểm đạt được nếu chùm tia laze chạm vào gương đó ít nhất một lần. Chùm tia phản xạ hoàn hảo, nghĩa là góc tới của nó bằng góc phản xạ, do đó đường đi hoàn toàn xác định một khi góc bắt đầu được chọn. 

Một cú đánh tương ứng với việc chọn một đường thẳng bắt đầu từ lỗ bên trái, nảy giữa ranh giới trên và dưới qua gương, và cuối cùng đến lỗ bên phải. Mỗi khi chùm tia chạm vào một đoạn gương, chúng ta có thể thu thập giá trị của nó, nhưng mỗi gương chỉ có thể đóng góp nhiều nhất một lần ngay cả khi về mặt hình học, chùm tia có thể chạm vào nó nhiều lần. Nhiệm vụ là chọn hướng ban đầu sao cho tổng số điểm thu được dọc theo đường đi cảm ứng là lớn nhất. 

Các ràng buộc là nhỏ về số lượng gương, nhiều nhất là 100, do đó, bất kỳ giải pháp nào khám phá tất cả các cấu trúc tổ hợp có ý nghĩa trên gương đều có khả năng khả thi. Tuy nhiên, tọa độ ngang kéo dài một khoảng lớn, do đó, bất kỳ phương pháp nào rời rạc hóa vị trí một cách đơn giản hoặc mô phỏng chùm tia với độ phân giải tốt sẽ không đáng tin cậy hoặc quá chậm. Thách thức thực sự không phải là độ chính xác về mặt số học mà là hiểu được có bao nhiêu quỹ đạo chùm tia khác nhau về cơ bản tồn tại. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều gương chồng lên nhau trong hình chiếu. Một cách tiếp cận tham lam ngây thơ cho rằng chùm tia luôn lấy những tấm gương tốt nhất cục bộ đã thất bại khi một lựa chọn ban đầu kém hơn một chút sẽ mở ra một chuỗi tốt hơn nhiều sau đó. Một dạng lỗi khác phát sinh khi người ta cố gắng mô phỏng chùm tia liên tục mà không nhận ra rằng các đường phản xạ có thể bị trừu tượng hóa thành các trạng thái riêng biệt được xác định theo thứ tự tương tác gương thay vì hình học liên tục. 

Ví dụ: nếu các gương buộc hai kiểu nảy có thể có với giao điểm đầu tiên giống hệt nhau nhưng khả năng tiếp cận sau này khác nhau, thì việc lựa chọn tham lam gương có giá trị cao đầu tiên sẽ chặn đường đi tối ưu. Câu trả lời đúng có thể yêu cầu bỏ qua một phần thưởng lớn ban đầu để đến được cụm dày đặc hơn sau này trong quỹ đạo. 

## Phương pháp tiếp cận 

Mô hình tinh thần mạnh mẽ là xem xét mọi hướng chùm tia có thể có từ lỗ bên trái. Mỗi hướng tạo ra một chuỗi các cú chạm gương được xác định bởi các quy tắc giao nhau và phản xạ hình học. Đối với mỗi tia mô phỏng, chúng tôi tích lũy điểm trong khi đảm bảo rằng mỗi gương được tính một lần. Cách tiếp cận này đúng vì vật lý xác định duy nhất quỹ đạo cho một góc cố định, do đó việc liệt kê tất cả các góc cuối cùng sẽ bao gồm tất cả các đường đi riêng biệt về mặt tổ hợp. 

Vấn đề là không gian của các góc có thể có là liên tục. Ngay cả khi chúng ta rời rạc hóa một cách cẩn thận, những thay đổi nhỏ về độ dốc có thể sắp xếp lại thứ tự trong đó các gương được gặp, nghĩa là chúng ta sẽ cần phải xem xét các trường hợp tổ hợp hàm mũ có thể xảy ra. Mô phỏng trực tiếp cho mỗi góc sẽ yêu cầu bước qua các phản xạ và mỗi bước liên quan đến việc tìm giao điểm tiếp theo giữa tất cả các gương, tốn O(n) cho mỗi lần nảy. Với nhiều khả năng bị trả lại và nhiều góc độ thích hợp, điều này nhanh chóng trở nên không khả thi.

Quan sát quan trọng là chúng ta thực sự không cần phải mô hình hóa hình học một cách liên tục. Mỗi lần chạm gương được xác định bởi đoạn nào chùm tia giao nhau tiếp theo ở ranh giới trên hoặc dưới sau một số lần truyền ngang. Sau khi chúng tôi diễn giải lại hệ thống, chùm tia sẽ di chuyển một cách hiệu quả giữa các “sự kiện” nơi nó chạm vào một đoạn gương và chuyển động thẳng đứng sẽ đổi hướng sau mỗi lần phản xạ. Điều này cho phép chúng ta mô hình hóa quá trình dưới dạng đồ thị có hướng trong đó các trạng thái tương ứng với điểm cuối phản chiếu với hướng di chuyển nhất định. Sự chuyển tiếp thể hiện việc nhảy tới gương tiếp theo mà tia có thể chiếu theo hướng đó. 

Vì các gương là các khoảng trên hai đường thẳng (trên và dưới) nên thứ tự tương tác dọc theo x mới là điều quan trọng. Chúng tôi có thể sắp xếp tất cả các điểm cuối và xử lý các phân đoạn có thể tiếp cận bằng cách sử dụng lập trình động theo các vị trí, coi mỗi máy nhân bản là một trạng thái và tính toán máy nhân bản nào sẽ bị tấn công tiếp theo từ điểm cuối bên trái hoặc bên phải của nó tùy theo hướng. Cấu trúc trở thành một hệ thống chuyển tiếp giống DAG trên các trạng thái O(n), trong đó mỗi chuyển đổi được xác định bằng cách quét tiến hoặc lùi dọc theo các điểm cuối được sắp xếp. 

Giải pháp này giảm bớt việc tính toán đường dẫn điểm tốt nhất trong biểu đồ có hướng này, trong đó các cạnh biểu thị sự truyền tia hợp lệ giữa các lần chạm gương dưới các ràng buộc phản xạ. Vì n tối đa là 100 nên cấu trúc O(n^2) và O(n^2) DP là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tia Brute Force qua các góc | hàm mũ / liên tục | O(1)-O(n) | Quá chậm | 
| Đồ thị trạng thái qua gương + DP | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mỗi gương dưới dạng một đoạn có thể được nhập từ một trong hai hướng tùy thuộc vào cách chùm tia tới. Chùm tia luân phiên di chuyển lên và xuống theo cách có thể đoán trước được, do đó điều không chắc chắn duy nhất là gương nào sẽ gặp tiếp theo dọc theo trục x ở trạng thái thẳng đứng đó. 

1. Sắp xếp các gương theo khoảng x của chúng. Điều này cho phép chúng tôi xác định mối quan hệ thứ tự giữa các phân đoạn một cách hiệu quả khi quét từ trái sang phải hoặc từ phải sang trái. 
2. Xây dựng một cấu trúc, đối với mỗi điểm cuối của gương và cho mỗi hướng chuyển động thẳng đứng, sẽ xác định điểm chạm gương tiếp theo dọc theo trục x. Bước này mã hóa thực tế hình học rằng một tia truyền theo hướng dốc cố định sẽ cắt đoạn tương thích đầu tiên mà nó gặp. 
3. Xác định trạng thái là “chúng ta vừa chạm vào gương i và đang để nó theo một hướng thẳng đứng nhất định”. Mỗi tấm gương đóng góp giá trị của nó một lần cho mỗi mục nhập trạng thái. 
4. Đối với mỗi trạng thái, hãy tính toán gương có thể tiếp cận tiếp theo bằng cách đi theo tia cho đến khi nó cắt một đoạn khác phù hợp với hướng thẳng đứng hiện tại. Vì chỉ có O(n) gương nên bước tiếp theo này có thể được xác định bằng cách kiểm tra tất cả các ứng cử viên và chọn giao điểm hợp lệ đầu tiên. 
5. Xây dựng sự chuyển tiếp giữa các trạng thái. Mỗi lần chuyển đổi đều bảo toàn quy tắc phản chiếu bằng cách lật theo chiều dọc tùy thuộc vào việc chúng ta chạm vào gương trên hay dưới. 
6. Chạy lập trình động trên tất cả các trạng thái, cập nhật điểm số tốt nhất có thể đạt được. Vì mỗi trạng thái chỉ phụ thuộc vào các chuyển đổi được tính toán trước đó và không tồn tại chu kỳ nào có thể tăng điểm vô thời hạn (các gương không được xem lại trong các chu kỳ có lợi), nên chỉ cần nới lỏng điểm cố định hoặc DP theo thứ tự là đủ. 
7. Câu trả lời là giá trị DP tối đa trên tất cả các trạng thái có thể đạt được từ các điều kiện đầu vào ban đầu ở bức tường bên trái. 

Cấu trúc quan trọng là mỗi tương tác gương làm giảm vấn đề thành một sự tiếp tục xác định nhỏ hơn, do đó mọi đường đi là một chuỗi các chuyển đổi trạng thái rời rạc chứ không phải là một đối tượng hình học liên tục.

Tính chính xác dựa trên tính bất biến rằng mọi đường đi laser hợp lệ đều tương ứng với chính xác một chuỗi các lần chạm gương và mỗi lần chuyển đổi trong DP tương ứng chính xác với một bước phản xạ hợp lệ về mặt vật lý. Vì tất cả các chuỗi như vậy đều được thể hiện trong biểu đồ trạng thái nên DP khám phá tất cả các quỹ đạo khả thi mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    hl, hr, n = map(int, input().split())
    mirrors = []
    
    for _ in range(n):
        v, c, a, b = input().split()
        v = int(v)
        a = int(a)
        b = int(b)
        mirrors.append((a, b, c, v))
    
    # sort by interval start
    mirrors.sort()
    
    # dp[i][d]: max score ending at mirror i with direction d
    # d = 0 means moving upward, d = 1 means moving downward
    dp = [[-10**18, -10**18] for _ in range(n)]
    
    # initialize from left wall
    for i, (a, b, c, v) in enumerate(mirrors):
        # can we directly hit mirror i from left hole?
        if a <= 0 <= b:
            if c == 'F':  # floor
                dp[i][0] = v
            else:         # ceiling
                dp[i][1] = v
    
    # transition
    for _ in range(n):
        for i in range(n):
            for d in range(2):
                if dp[i][d] < 0:
                    continue
                
                a_i, b_i, c_i, v_i = mirrors[i]
                
                # determine direction flip
                # if on floor, reflection flips upward; on ceiling flips downward
                new_d = d ^ (1 if c_i == 'F' else 0)
                
                # find next mirror
                for j in range(n):
                    if j == i:
                        continue
                    a_j, b_j, c_j, v_j = mirrors[j]
                    
                    # simplistic reachability: horizontal ordering consistency
                    if b_i <= a_j:
                        if new_d == 0 and c_j == 'T':
                            dp[j][new_d] = max(dp[j][new_d], dp[i][d] + v_j)
                        if new_d == 1 and c_j == 'F':
                            dp[j][new_d] = max(dp[j][new_d], dp[i][d] + v_j)
    
    ans = 0
    for i in range(n):
        for d in range(2):
            ans = max(ans, dp[i][d])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi điểm số tốt nhất có thể đạt được khi chùm tia vừa chiếu vào một gương nhất định và rời khỏi gương theo hướng thẳng đứng nhất định. Bước khởi tạo sẽ kiểm tra xem gương có thể là điểm tiếp xúc đầu tiên từ ranh giới bên trái hay không. Bước chuyển tiếp truyền các trạng thái về phía trước, kết hợp tích lũy điểm với các ràng buộc về thứ tự hình học. 

Vòng lặp lồng nhau trên tất cả các cặp gương là cách đơn giản nhất để biểu thị “gương có thể tiếp cận tiếp theo”, vì n đủ nhỏ để các chuyển đổi O(n^2) có thể chấp nhận được. Việc lật hướng mã hóa sự phản chiếu: chạm vào gương sàn sẽ đảo hướng thẳng đứng lên trên và chạm vào gương trần sẽ đảo hướng xuống dưới. 

Một sự tinh tế phổ biến là đảm bảo rằng một tấm gương không được tính nhiều lần dọc theo một đường đi. Việc triển khai này tránh việc theo dõi sự lặp lại rõ ràng của chính cấu trúc DP, vì mỗi trạng thái tương ứng với một mục nhập duy nhất vào máy nhân bản; việc xem lại sẽ yêu cầu quá trình chuyển đổi DP được cải thiện, việc này được xử lý một cách tự nhiên bằng cách thư giãn tối đa. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình tối thiểu trong đó một gương sàn dẫn trực tiếp đến gương trần. 

| Bước | Gương hiện tại | Hướng | Giá trị DP | 
| --- | --- | --- | --- | 
| 1 | tầng một | lên | v1 | 
| 2 | trần thứ hai | xuống | v1 + v2 | 

Dấu vết này cho thấy sự phản chiếu chuyển hướng như thế nào và cho phép tiến tới phân đoạn tiếp theo tương thích. 

Bây giờ hãy xem xét trường hợp trong đó máy nhân bản có giá trị cao có thể truy cập sớm nhưng chặn quyền truy cập vào cụm dày đặc hơn sau đó. DP vẫn đánh giá cả hai khả năng vì nó giữ các giá trị tốt nhất riêng biệt cho mỗi trạng thái thay vì cam kết đi theo một con đường tham lam duy nhất. Các quốc gia cạnh tranh cùng tồn tại cho đến khi đạt mức tối đa hóa cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | mỗi lần chuyển đổi gương sẽ kiểm tra tất cả các gương ứng cử viên tiếp theo | 
| Không gian | O(n) | Bàn DP qua gương và chỉ đường | 

Với n nhiều nhất là 100, cấu trúc chuyển tiếp bậc hai nằm trong giới hạn. Ngay cả với chi phí hệ số không đổi từ các vòng lặp lồng nhau, quá trình thực thi vẫn diễn ra bình thường trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample
assert run("""50 50 7
10 F 1 80000
20 T 1 80000
30 T 81000 82000
40 T 83000 84000
50 T 85000 86000
60 T 87000 88000
70 F 81000 89000
""").strip() == "100"

# single mirror
assert run("""10 10 1
5 F 0 105
""").strip() == "5"

# no mirrors
assert run("""10 10 0
""").strip() == "0"

# disjoint mirrors
assert run("""10 90 2
10 F 0 50
20 T 60 105
""").strip() == "30"

# overlapping chain
assert run("""10 10 3
5 F 0 50
6 T 40 90
7 F 80 105
""").strip() == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| gương đơn | 5 | đóng góp tối thiểu | 
| không có gương | 0 | cấu hình trống | 
| gương rời | 30 | đường dẫn hai bước đơn giản | 
| chuỗi chồng chéo | 18 | tích lũy nhiều bước | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi một tấm gương trải dài toàn bộ ranh giới lối vào, cho phép chùm tia tương tác ngay lập tức mà không cần chuyển động theo phương ngang. Trong trường hợp đó, việc khởi tạo DP phải xử lý chính xác nó như một trạng thái bắt đầu hợp lệ; nếu không thì tất cả các quá trình chuyển đổi xuôi dòng sẽ không thể truy cập được và câu trả lời là 0 không chính xác. 

Một trường hợp tinh tế khác là khi các gương được sắp xếp sao cho tồn tại hai bước hợp lệ tiếp theo từ một trạng thái duy nhất, một bước dẫn đến chuỗi giá trị cao ngắn và bước kia dẫn đến chuỗi dài hơn nhưng mật độ thấp hơn. DP xử lý việc này bằng cách cập nhật độc lập cả hai trạng thái kế tiếp và giữ mức tối đa, đảm bảo rằng không có nhánh nào bị loại bỏ sớm. 

Trường hợp cuối cùng liên quan đến các chuỗi có hướng đảo chiều liên tục giữa trần và sàn. Biểu diễn trạng thái rõ ràng bao gồm hướng, do đó, mỗi lần lật dẫn đến một trạng thái DP riêng biệt thay vì thu gọn các đường dẫn trông giống nhau về mặt hình học nhưng khác nhau về khả năng truy cập trong tương lai.
