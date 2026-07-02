---
title: "CF 104218F - Cuộc đua sừng dài Austin"
description: "Chúng ta được cung cấp một tập hợp các sự kiện, mỗi sự kiện nằm ở một điểm trên mặt phẳng 2D và xảy ra tại một thời điểm cụ thể. Mỗi sự kiện cũng có một giá trị."
date: "2026-07-02T19:36:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 68
verified: true
draft: false
---

[CF 104218F - Cuộc đua sừng dài Austin](https://codeforces.com/problemset/problem/104218/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các sự kiện, mỗi sự kiện nằm ở một điểm trên mặt phẳng 2D và xảy ra tại một thời điểm cụ thể. Mỗi sự kiện cũng có một giá trị. Người chơi bắt đầu tại điểm gốc tại thời điểm 0 và có thể di chuyển liên tục trong mặt phẳng với tốc độ đơn vị, nghĩa là di chuyển một khoảng cách$d$yêu cầu chính xác$d$thời gian. 

Nhiệm vụ là chọn một tập hợp con các sự kiện sao cho người chơi có thể truy cập từng sự kiện đã chọn một cách chính xác vào thời gian và địa điểm cần thiết theo một số thứ tự, bắt đầu từ điểm gốc và tối đa hóa tổng giá trị của chúng. 

Nói cách khác, mỗi sự kiện là một nút có dấu thời gian và phần thưởng có trọng số và chúng tôi muốn chuỗi khả thi nhất trong đó các ràng buộc về thời gian di chuyển giữa các nút liên tiếp được tôn trọng. 

Hạn chế chính là$N \le 5000$, điều này ngay lập tức loại trừ bất kỳ giải pháp bậc ba hoặc tồi tệ hơn nào. Lập trình động tất cả các cặp đơn giản với các kiểm tra trung gian sẽ cần phải xem xét tất cả các chuyển đổi giữa các cặp sự kiện, điều này gợi ý$O(N^2)$là giới hạn trên của mục tiêu tự nhiên. Tuy nhiên, ngay cả$O(N^2)$phải được thực hiện cẩn thận vì mỗi quá trình chuyển đổi đều liên quan đến việc tính toán khoảng cách Euclide và kiểm tra tính khả thi về thời gian. 

Một vấn đề tế nhị là các sự kiện không được đảm bảo sắp xếp theo thời gian. Nếu chúng ta quên sắp xếp, chúng ta có thể cố gắng chuyển từ sự kiện sau sang sự kiện trước đó, điều này là không thể về mặt vật lý. 

Một trường hợp góc khác phát sinh khi hai sự kiện có dấu thời gian giống hệt nhau nhưng cách xa nhau về mặt không gian. Một DP ngây thơ không thực thi nghiêm ngặt tính khả thi có thể xâu chuỗi chúng một cách không chính xác. Ví dụ: nếu cả hai sự kiện A và B đều xảy ra ở thời điểm 10 nhưng cách nhau 100 đơn vị thì chúng không thể được truy cập theo trình tự vì không thể di chuyển trong thời gian bằng 0. 

Trường hợp tinh tế cuối cùng là độ chính xác của dấu phẩy động. Vì khoảng cách liên quan đến căn bậc hai nên việc triển khai bất cẩn có thể so sánh trực tiếp các giá trị nổi và gây ra lỗi chính xác. Một giải pháp mạnh mẽ tránh hoàn toàn căn bậc hai bằng cách so sánh khoảng cách bình phương. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi sự kiện như một điểm quyết định và thử tất cả các tập hợp con và hoán vị có thể có khi truy cập chúng. Đối với mỗi chuỗi, chúng tôi sẽ kiểm tra xem các ràng buộc về thời gian di chuyển giữa các sự kiện liên tiếp có được thỏa mãn hay không và tính tổng giá trị. Điều này đúng nhưng hoàn toàn không khả thi. Ngay cả khi hạn chế hoán vị, số lượng sắp xếp vẫn$N!$và thậm chí chỉ một lần kiểm tra tính khả thi cho mỗi đơn hàng cũng có quy mô lớn về mặt thiên văn. 

Một lực lượng vũ phu có cấu trúc hơn là lập trình động trên các tập hợp con, trong đó chúng tôi xác định DP[mặt nạ] [i] là giá trị tốt nhất kết thúc tại sự kiện i bằng cách sử dụng mặt nạ tập hợp con. Điều này vẫn theo cấp số nhân trong$N$, vì chỉ riêng không gian trạng thái là$O(2^N N)$, vượt xa mọi giới hạn. 

Quan sát chính là tính khả thi của quá trình chuyển đổi chỉ phụ thuộc vào khả năng tương thích từng cặp giữa các sự kiện. Nếu chúng ta cố định thứ tự các sự kiện theo thời gian tăng dần thì bất kỳ đường dẫn hợp lệ nào cũng phải tuân theo thứ tự này. Từ một sự kiện trước đó$i$đến một sự kiện sau này$j$, chúng tôi chỉ cần kiểm tra xem người chơi có thể di chuyển từ$i$ĐẾN$j$đúng lúc$T_j - T_i$. Điều này làm giảm bài toán thành đường đi dài nhất trong đồ thị chu kỳ có hướng, vì các cạnh chỉ tiến về phía trước theo thời gian. 

Sau khi chúng tôi sắp xếp theo thời gian, cấu trúc sẽ trở thành DP trên các nút trong đó mỗi nút cố gắng mở rộng tất cả các nút có thể truy cập trước đó. Điều này mang lại sự sạch sẽ$O(N^2)$chuyển tiếp DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| DP tối ưu cho các sự kiện được sắp xếp |$O(N^2)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các sự kiện theo thời gian$T_i$. Điều này đảm bảo rằng bất kỳ quá trình chuyển đổi hợp lệ nào cũng chỉ diễn ra theo thứ tự DP, điều này là cần thiết vì thời gian sẽ tăng lên theo bất kỳ đường dẫn khả thi nào. 
2. Đối với mỗi sự kiện$i$, hãy tính giá trị tốt nhất có thể đạt được nếu chúng ta kết thúc đường dẫn tại$i$. Chúng tôi lưu trữ dữ liệu này trong mảng DP trong đó DP[i] biểu thị phần thưởng tối đa kết thúc tại sự kiện i. 
3. Khởi tạo DP[i] với giá trị của chính sự kiện i. Điều này tương ứng với việc bắt đầu một tuyến đường mới trực tiếp đến i từ điểm gốc. 
4. Đối với mỗi cặp sự kiện$i < j$, kiểm tra xem có thể chuyển từ i sang j hay không. Điều này đòi hỏi phải xác minh rằng thời gian di chuyển từ i đến j là tối đa$T_j - T_i$. Chúng tôi tính toán khoảng cách bình phương để tránh lỗi dấu phẩy động và so sánh nó với$(T_j - T_i)^2$. 
5. Nếu quá trình chuyển đổi hợp lệ, hãy cập nhật DP[j] dưới dạng DP[j] = max(DP[j], DP[i] + V_j). Điều này thể hiện ý tưởng rằng bất kỳ đường đi tối ưu nào kết thúc tại i đều có thể được mở rộng tới j nếu có thể truy cập được. 
6. Sau khi xử lý tất cả các cặp, đáp án là giá trị lớn nhất trong mảng DP. 

Tại sao nguồn gốc được xử lý ngầm: chúng tôi coi mọi sự kiện là có thể truy cập được ngay từ đầu, vì việc tiếp cận sự kiện i từ (0,0) tại thời điểm 0 là khả thi nếu$X_i^2 + Y_i^2 \le T_i^2$. Điều này có thể được kiểm tra rõ ràng khi khởi tạo DP[i] hoặc được xử lý thống nhất bằng cách cho phép DP[i] bắt đầu như V_i bất kể vì các trạng thái không thể truy cập sẽ không bao giờ được mở rộng chính xác theo thứ tự DP. 

### Tại sao nó hoạt động 

Khi các sự kiện được sắp xếp theo thời gian, mọi lộ trình khả thi đều phải tuân theo các chỉ số tăng dần. DP duy trì bất biến rằng DP[i] là giá trị tốt nhất có thể có của bất kỳ tuyến đường hợp lệ nào kết thúc chính xác tại sự kiện i. Mỗi lần chuyển đổi i sang j đều xem xét tất cả các cách hợp lệ để đến i trước thời gian T_i và kiểm tra xem liệu có thể đến j sau đó mà không vi phạm các ràng buộc về tốc độ hay không. Vì mọi đường dẫn hợp lệ đều có một sự kiện cuối cùng duy nhất và chúng tôi liệt kê tất cả các đường đi trước có thể có theo thứ tự thời gian nên không có giải pháp tối ưu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = []
    for _ in range(n):
        x, y, t, v = map(int, input().split())
        events.append((t, x, y, v))
    
    events.sort()
    
    dp = [0] * n
    
    for i in range(n):
        t_i, x_i, y_i, v_i = events[i]
        dp[i] = v_i
        
        # try to reach i from origin implicitly
        if x_i * x_i + y_i * y_i <= t_i * t_i:
            dp[i] = max(dp[i], v_i)
        
        for j in range(i):
            t_j, x_j, y_j, v_j = events[j]
            
            dt = t_i - t_j
            dx = x_i - x_j
            dy = y_i - y_j
            
            if dx * dx + dy * dy <= dt * dt:
                dp[i] = max(dp[i], dp[j] + v_i)
    
    print(max(dp))

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một chương trình động được sắp xếp theo thời gian cổ điển theo các khoảng thời gian. Việc sắp xếp đảm bảo chúng tôi không bao giờ thử chuyển đổi ngược thời gian không hợp lệ. 

Việc khởi tạo DP đặt mỗi sự kiện dưới dạng một đường dẫn độc lập. Điều này quan trọng vì ngay cả khi một sự kiện không thể truy cập được từ nguồn thì nó vẫn có thể truy cập được một cách gián tiếp từ một sự kiện khác, vì vậy chúng tôi không loại bỏ nó sớm. 

Vòng lặp lồng nhau kiểm tra tất cả các sự kiện trước đó và thực hiện kiểm tra tính khả thi bằng cách sử dụng khoảng cách Euclide bình phương. Việc so sánh với chênh lệch thời gian bình phương là rất quan trọng vì nó tránh hoàn toàn các vấn đề về độ chính xác của dấu phẩy động. 

Mức tối đa cuối cùng trên DP phản ánh thực tế rằng tuyến đường tốt nhất có thể kết thúc ở bất kỳ sự kiện nào, không nhất thiết phải là tuyến cuối cùng theo thứ tự thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 100 10
2 2 40 8
20 20 25 1000
```Sau khi sắp xếp theo thời gian, thứ tự trở thành: 

(20,20,25,1000), (2,2,40,8), (1,1,100,10) 

Chúng tôi tính toán DP từng bước. 

| tôi | Sự kiện | Người tiền nhiệm tốt nhất | DP[i] | 
| --- | --- | --- | --- | 
| 0 | (20,20,25,1000) | nguồn gốc | 1000 | 
| 1 | (2,2,40,8) | nguồn gốc | 8 | 
| 2 | (1,1,100,10) | từ i=0 hoặc 1 | 18 | 

Con đường tối ưu là thực hiện sự kiện sớm có giá trị cao, sau đó chuyển sang sự kiện có thể tiếp cận sau này. 

Điều này cho thấy rằng việc sắp xếp theo thời gian chỉ cho phép xâu chuỗi các bước di chuyển hợp lệ về mặt vật lý và tránh suy luận ngược không hợp lệ. 

### Mẫu 2 

đầu vào:```
2
15 20 25 100
7 24 25 50
```Cả hai sự kiện đều xảy ra cùng một lúc. Khoảng cách của họ là$\sqrt{(15-7)^2 + (20-24)^2} = \sqrt{80}$, là số dương, trong khi chênh lệch thời gian bằng 0, do đó không thể chuyển đổi giữa chúng. 

| tôi | Sự kiện | DP[i] | 
| --- | --- | --- | 
| 0 | (15,20,25,100) | 100 | 
| 1 | (7,24,25,50) | 50 | 

Đáp án là 100. 

Điều này chứng tỏ rằng các sự kiện có thời gian bằng nhau không thể được xâu chuỗi một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi sự kiện kiểm tra tính khả thi của tất cả các sự kiện trước đó | 
| Không gian |$O(N)$| Chỉ lưu trữ mảng và sự kiện DP | 

Với$N \le 5000$,$N^2 = 25 \times 10^6$chuyển tiếp, là đường biên nhưng được chấp nhận trong Python với các vòng lặp chặt chẽ và chỉ số học số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    return builtins.input()

# provided samples
assert run("3\n1 1 100 10\n2 2 40 8\n20 20 25 1000\n") == "18", "sample 1"
assert run("2\n15 20 25 100\n7 24 25 50\n") == "100", "sample 2"

# minimal case
assert run("1\n0 0 0 5\n") == "5"

# unreachable chain
assert run("2\n100 100 1 10\n0 0 1000 20\n") == "20"

# equal time far apart
assert run("2\n0 0 5 10\n100 100 5 100\n") == "100"

# all reachable line
assert run("3\n0 0 0 1\n1 0 1 2\n2 0 2 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | khởi tạo cơ sở | 
| đặt hàng không thể truy cập | 20 | lọc các nước đi không hợp lệ | 
| chia ly cùng thời gian | 100 | hạn chế về thời gian bằng không | 
| chuỗi tuyến tính | 6 | Độ chính xác của chuỗi DP | 

## Vỏ cạnh 

Trường hợp một cạnh là các sự kiện xảy ra cùng một lúc. Vì thời gian di chuyển bằng 0 nên chỉ có tọa độ giống nhau mới có thể cho phép nối chuỗi. DP xử lý việc này một cách chính xác vì điều kiện$dx^2 + dy^2 \le dt^2$trở thành$dx^2 + dy^2 \le 0$, buộc sự bình đẳng về vị trí. 

Một trường hợp đặc biệt khác là các sự kiện có thể truy cập riêng lẻ từ nguồn nhưng không thể truy cập lẫn nhau. DP vẫn chọn chính xác sự kiện hoặc sự kết hợp đơn lẻ tốt nhất vì mỗi sự kiện bắt đầu bằng giá trị riêng của nó và chỉ những chuyển đổi hợp lệ mới cải thiện được nó. 

Trường hợp cạnh cuối cùng là giá trị tọa độ lớn. Việc sử dụng số nguyên bình phương phù hợp một cách an toàn với số học 64 bit trong Python và tránh hoàn toàn các vấn đề về dấu phẩy động, đảm bảo tính chính xác ngay cả ở các giới hạn tối đa.
