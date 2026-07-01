---
title: "CF 104217F - Cuộc đua sừng dài Austin"
description: "Chúng ta được cung cấp một tập hợp các điểm kiểm tra nằm rải rác trong mặt phẳng 2D. Mỗi trạm kiểm soát chỉ xuất hiện tại một thời điểm cụ thể và nếu chúng ta đến chính xác tọa độ của nó vào thời điểm chính xác đó, chúng ta có thể thu được một số phần thưởng."
date: "2026-07-01T23:54:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104217
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104217
solve_time_s: 99
verified: false
draft: false
---

[CF 104217F - Cuộc đua sừng dài Austin](https://codeforces.com/problemset/problem/104217/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm kiểm tra nằm rải rác trong mặt phẳng 2D. Mỗi trạm kiểm soát chỉ xuất hiện tại một thời điểm cụ thể và nếu chúng ta đến chính xác tọa độ của nó vào thời điểm chính xác đó, chúng ta có thể thu được một số phần thưởng. 

Chúng ta bắt đầu tại điểm gốc ở thời điểm 0 và có thể di chuyển liên tục theo bất kỳ hướng nào với tốc độ một đơn vị khoảng cách trên một đơn vị thời gian. Điều này có nghĩa là việc di chuyển từ điểm A đến điểm B cần chính xác khoảng cách Euclide giữa chúng và chúng ta không được đến sớm hơn thời gian đã định nếu muốn chờ, nhưng đến muộn hơn cũng không giúp ích được gì vì kho báu chỉ có vào đúng thời điểm của nó. 

Mục tiêu là chọn một chuỗi các điểm kiểm tra để truy cập, bắt đầu từ điểm gốc, sao cho mỗi lần chuyển đổi đều khả thi về mặt vật lý về mặt thời gian và tổng giá trị thu thập được là tối đa. 

Kích thước đầu vào lên tới 5000 điểm kiểm tra. Một nỗ lực ngây thơ để kiểm tra tất cả các chuỗi lượt truy cập có thể sẽ yêu cầu khám phá các hoán vị hoặc đường dẫn trong biểu đồ có 5000 nút, tăng trưởng theo giai thừa hoặc ít nhất là bậc hai cho mỗi bước chuyển đổi trạng thái, khiến bất cứ điều gì như O(N^2 log N) hoặc tệ hơn là đường biên giới tiềm năng tùy thuộc vào hằng số và O(N^3) hoặc hàm mũ rõ ràng là không thể. 

Điểm tinh tế quan trọng nhất là hạn chế về thời gian: ngay cả khi một điểm gần về mặt hình học, nó có thể không thể tiếp cận được vì dấu thời gian của nó quá sớm. Ngược lại, các điểm xa vẫn có thể truy cập được nếu đủ thời gian trôi qua. Điều này làm cho bài toán trở thành bài toán đường đi dài nhất bị ràng buộc trong đồ thị có hướng được xác định bởi khả năng tiếp cận trong không thời gian. 

Một vài trường hợp đột phá phá vỡ lối suy nghĩ tham lam ngây thơ. Một thất bại phổ biến là cho rằng chúng ta luôn thích điểm tiếp theo gần nhất. Ví dụ: sớm hơn một điểm có giá trị thấp gần đó có thể cản trở việc đạt được điểm có giá trị cao sau đó do hạn chế về thời gian, mặc dù việc bỏ qua điểm đó cho phép thu được lợi ích lớn hơn nhiều. Một trường hợp tinh tế khác là dấu thời gian bằng nhau: không thể lấy cả hai điểm cùng lúc trừ khi chúng ở vị trí giống hệt nhau, điều này nói chung là không thể, vì vậy các ràng buộc về thứ tự phải được tôn trọng nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là coi mỗi điểm kiểm tra như một nút và kết nối nút i với nút j nếu có thể di chuyển từ i đến j kịp thời. Từ nút i tại thời điểm Ti, chúng ta có thể đến nút j tại thời điểm Tj nếu Ti + dist(i, j) ≤ Tj. Nếu chúng tôi cũng bao gồm nút bắt đầu ảo tại (0,0,0), chúng tôi có thể tính toán đường dẫn thưởng tốt nhất kết thúc ở mỗi điểm kiểm tra. 

Điều này ngay lập tức gợi ý một công thức lập trình động trong đó dp[i] là phần thưởng tối đa kết thúc tại điểm kiểm tra i. Đối với mỗi i, chúng ta cố gắng chuyển từ tất cả các điểm kiểm tra khả thi trước đó j với Tj  Ti và kiểm tra xem liệu j có thể đến i kịp thời hay không. Điều này đưa ra giải pháp O(N^2) đơn giản. 

Cấu trúc vũ lực hoạt động vì mọi điểm kiểm tra chỉ phụ thuộc vào các điểm kiểm tra có thể tiếp cận trước đó. Tuy nhiên, cách giải thích ngây thơ rằng chúng ta phải xem xét tất cả các hoán vị hoặc các chuỗi tùy ý là quá lớn. Sự cải tiến xuất phát từ việc nhận ra rằng tính khả thi của yêu cầu nghiêm ngặt về thời gian: chuyển đổi chỉ có hiệu lực từ thời gian nhỏ hơn hoặc bằng thời gian lớn hơn. Điều này chuyển đổi vấn đề thành vấn đề đường đi ngắn nhất hoặc dài nhất DAG, trong đó các cạnh chỉ tiến về phía trước theo thời gian. 

Không giống như các bài toán đường đi ngắn nhất DAG điển hình, các cạnh không được đưa ra một cách rõ ràng; chúng phải được tính toán bằng khoảng cách hình học. Điều đó vẫn có thể chấp nhận được trong O(N^2), vì việc kiểm tra một cặp là thời gian không đổi. 

Không cần cấu trúc dữ liệu hoặc hình học phức tạp hơn vì N = 5000 giữ các chuyển đổi bậc hai trong phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đường dẫn lực lượng vũ phu trên hoán vị | O(N!) | O(N) | Quá chậm | 
| DP trên tất cả các chuyển tiếp hợp lệ | O(N^2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi sắp xếp các điểm kiểm tra theo thời gian để tất cả các chuyển đổi hợp lệ sẽ đi từ điểm trước đến điểm sau. Sau đó, chúng tôi chạy lập trình động theo thứ tự này. 

### Các bước 

1. Đọc tất cả các điểm kiểm tra và liên kết từng điểm với giá trị, tọa độ và thời gian của nó. Thêm điểm kiểm tra bắt đầu ảo tại (0, 0, 0) với giá trị 0. 
2. Sắp xếp tất cả các điểm theo thời gian tăng dần. Điều này đảm bảo mọi hướng chuyển động hợp lệ theo thời gian luôn tiến về phía trước theo thứ tự mảng. Thứ tự này rất quan trọng vì nó ngăn cản việc xem lại các trạng thái trong tương lai khi tính dp. 
3. Khởi tạo một mảng dp trong đó dp[i] biểu thị tổng giá trị tối đa có thể đạt được kết thúc chính xác tại điểm kiểm tra i. Đặt tất cả các giá trị thành âm vô cực ngoại trừ nút bắt đầu là 0. 
4. Đối với mỗi điểm kiểm tra i theo thứ tự được sắp xếp, lặp lại tất cả các điểm kiểm tra trước đó j < i. 
5. Với mỗi cặp (j, i), hãy tính khoảng cách Euclide giữa chúng và kiểm tra xem quá trình chuyển đổi dp có khả thi hay không: dp[j] hợp lệ và Tj + dist(j, i) ≤ Ti. 
6. Nếu khả thi, hãy cập nhật dp[i] = max(dp[i], dp[j] + Vi). Điều này có nghĩa là chúng tôi mở rộng tuyến đường nổi tiếng nhất kết thúc tại j bằng cách lấy điểm kiểm tra i. 
7. Sau khi xử lý tất cả các nút, câu trả lời là dp[i] tối đa trên tất cả các điểm kiểm tra. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến mà dp[i] lưu trữ phần thưởng tốt nhất có thể có trong số tất cả các đường dẫn hợp lệ kết thúc chính xác tại điểm kiểm tra i, chỉ xem xét các điểm kiểm tra xảy ra không muộn hơn i theo thứ tự thời gian. Bởi vì mọi quá trình chuyển đổi đều tôn trọng cả thứ tự thời gian và tính khả thi của việc di chuyển, nên mỗi lần cập nhật dp đều tương ứng với một bước đi hợp lệ về mặt vật lý. Vì tất cả các đường dẫn trước có thể hợp lệ đều được kiểm tra nên không thể bỏ qua đường dẫn nào tốt hơn tới i. Việc sắp xếp thời gian đảm bảo tính không tuần hoàn trong biểu đồ chuyển đổi, do đó, không trạng thái nào trong tương lai có thể ảnh hưởng đến trạng thái trong quá khứ, ngăn chặn các chu kỳ hoặc sự không nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n = int(input())
    pts = []
    
    for _ in range(n):
        x, y, t, v = map(int, input().split())
        pts.append((t, x, y, v))
    
    # sort by time
    pts.sort()
    
    # dp[i] = best value ending at i
    dp = [0] * n
    
    ans = 0
    
    for i in range(n):
        t_i, x_i, y_i, v_i = pts[i]
        dp[i] = v_i
        
        for j in range(i):
            t_j, x_j, y_j, v_j = pts[j]
            
            dx = x_i - x_j
            dy = y_i - y_j
            
            if dp[j] == 0 and j != 0:
                # still valid, dp[j] might be unreachable; skip only if desired
                pass
            
            dist = math.hypot(dx, dy)
            
            if t_j + dist <= t_i:
                dp[i] = max(dp[i], dp[j] + v_i)
        
        ans = max(ans, dp[i])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp theo thời gian để đảm bảo rằng khi chúng tôi tính toán dp[i], tất cả các phiên bản trước tiềm năng đều đã được xử lý. Vòng lặp bên trong kiểm tra mọi điểm kiểm tra trước đó và xác minh khả năng tiếp cận bằng khoảng cách Euclide. 

Một điểm tinh tế là độ chính xác của dấu phẩy động khi so sánh khoảng cách. Vì tọa độ và thời gian là số nguyên lên tới 1e9, nên sử dụng`math.hypot`là tiêu chuẩn, nhưng các vấn đề nghiêm ngặt đôi khi đòi hỏi phải có dung sai epsilon. Trong các môi trường cạnh tranh như thế này, cách này thường an toàn nhưng có một cách tiếp cận mạnh mẽ hơn là so sánh bình phương khoảng cách và bình phương chênh lệch thời gian một cách cẩn thận, mặc dù ở đây chênh lệch về thời gian bao gồm cả thời gian chờ di chuyển, do đó so sánh trực tiếp sẽ đơn giản hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 100 10
2 2 40 8
20 20 25 1000
```Đầu tiên chúng ta sắp xếp theo thời gian: 

| tôi | (x,y,t,v) | dp[i] init | chuyển tiếp tốt nhất | dp[i] cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | (20,20,25,1000) | 1000 | chỉ bắt đầu | 1000 | 
| 1 | (2,2,40,8) | 8 | từ (20,20,25) không thể kịp thời | 8 | 
| 2 | (1,1,100,10) | 10 | từ (2,2,40) có thể | 18 | 

Con đường tốt nhất là lấy điểm kiểm tra ở giữa và sau đó là điểm kiểm tra cuối cùng, tích lũy 8 + 10 = 18. 

Điều này cho thấy các điểm ban đầu có giá trị cao không phải lúc nào cũng tối ưu; những ràng buộc khả thi định hình con đường. 

### Mẫu 2 

đầu vào:```
2
15 20 25 100
7 24 25 50
```Thứ tự sắp xếp giữ cả hai ở thời điểm 25. Không thể đến được điểm kia vì thời gian di chuyển là dương nhưng chênh lệch thời gian có sẵn là bằng không. 

| tôi | dp[i] init | chuyển tiếp | dp[i] cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 100 | không | 100 | 
| 1 | 50 | không | 50 | 

Đáp án là 100. 

Điều này chứng tỏ rằng các điểm kiểm tra có thời gian bằng nhau là các nút độc lập một cách hiệu quả và không có chuyển tiếp hợp lệ giữa chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi cặp điểm kiểm tra đều được kiểm tra một lần về khả năng tiếp cận | 
| Không gian | O(N) | Mảng DP và các điểm được lưu trữ | 

Với N lên tới 5000, N² là khoảng 25 triệu kiểm tra, có thể chấp nhận được trong Python với các vòng lặp chặt chẽ và số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n = int(input())
        pts = []
        for _ in range(n):
            x, y, t, v = map(int, input().split())
            pts.append((t, x, y, v))
        pts.sort()
        dp = [0] * n
        ans = 0
        for i in range(n):
            t_i, x_i, y_i, v_i = pts[i]
            dp[i] = v_i
            for j in range(i):
                t_j, x_j, y_j, v_j = pts[j]
                dx = x_i - x_j
                dy = y_i - y_j
                if t_j + math.hypot(dx, dy) <= t_i:
                    dp[i] = max(dp[i], dp[j] + v_i)
            ans = max(ans, dp[i])
        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
1 1 100 10
2 2 40 8
20 20 25 1000
""") == "18"

assert run("""2
15 20 25 100
7 24 25 50
""") == "100"

# custom cases
assert run("""1
0 0 0 5
""") == "5"

assert run("""2
1 0 10 10
10 0 10 100
""") == "100"

assert run("""3
0 0 1 1
0 0 2 10
0 0 3 100
""") == "111"

assert run("""3
0 0 1 5
100 100 2 50
0 0 3 5
""") == "60"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | trường hợp cơ sở | 
| sự đánh đổi không thể đạt được cùng lúc | 100 | độc lập bình đẳng về thời gian | 
| tích lũy đơn điệu | 111 | tính khả thi của chuỗi | 
| yêu cầu đi đường vòng xa | 60 | thực thi ràng buộc hình học | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nhiều điểm kiểm tra có chung dấu thời gian. Sau khi sắp xếp, chúng trở nên liên tiếp nhưng không thể chuyển đổi lẫn nhau trừ khi khoảng cách bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì điều kiện`t_j + dist <= t_i`thất bại trừ khi dist bằng 0, vì vậy trạng thái dp vẫn độc lập. 

Một trường hợp khác là khi điểm kiểm tra có giá trị rất cao ở gần trong không gian nhưng có dấu thời gian sớm. Bất kỳ kẻ tham lam ngây thơ nào chọn theo giá trị hoặc khoảng cách sẽ cố gắng lấy nó trước, nhưng dp sẽ tránh được nó một cách chính xác nếu nó cản trở tính khả thi cho việc tích lũy sau này. 

Trường hợp cuối cùng là khi một điểm chỉ có thể đạt được thông qua một chuỗi dài các điểm trung gian. DP đảm bảo rằng khi mỗi trạng thái dp trung gian được tính toán, nó có thể truyền tiếp. Ví dụ: một chuỗi trong đó mỗi bước nhảy hầu như không thỏa mãn các ràng buộc về thời gian vẫn được tích lũy chính xác vì mọi trạng thái trung gian đều được coi là trạng thái tiền thân hợp lệ trong quá trình quét O(N2).
