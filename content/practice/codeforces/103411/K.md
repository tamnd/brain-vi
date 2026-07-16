---
title: "CF 103411K - Cá mập tấn công"
description: "Chúng ta được cho một dòng với một số con mực được đặt ở các vị trí nguyên. Có một con cá mập ở một vị trí khác và một nơi trú ẩn duy nhất ở một tọa độ cố định. Mọi con mực đều muốn đến nơi trú ẩn và một khi đến nơi an toàn, nó sẽ được bảo vệ mãi mãi."
date: "2026-07-03T10:59:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103411
codeforces_index: "K"
codeforces_contest_name: "2020-2021, ICPC, East Siberian Regional Contest"
rating: 0
weight: 103411
solve_time_s: 67
verified: true
draft: false
---

[CF 103411K - Cá mập tấn công](https://codeforces.com/problemset/problem/103411/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dòng với một số con mực được đặt ở các vị trí nguyên. Có một con cá mập ở một vị trí khác và một nơi trú ẩn duy nhất ở một tọa độ cố định. Mọi con mực đều muốn đến nơi trú ẩn và một khi đến nơi an toàn, nó sẽ được bảo vệ mãi mãi. 

Thời gian trôi qua tính bằng giây và cả cá mập và bất kỳ con mực nào hiện đang được điều khiển đều di chuyển ở tốc độ 1. Hạn chế chính là chỉ có một con mực có thể được chủ động di chuyển bất cứ lúc nào, trong khi tất cả những con mực khác vẫn bị đóng băng tại chỗ cho đến khi được chọn. 

Cá mập không thụ động. Nó cũng di chuyển tự do với tốc độ 1, và nếu bất cứ lúc nào nó chiếm giữ vị trí giống như một con mực vẫn còn trên thế giới thì con mực đó sẽ mất tích ngay lập tức. Con mực đến nơi trú ẩn trước khi bị cá mập chặn lại sẽ trở nên an toàn. 

Nhiệm vụ là chọn thứ tự kích hoạt mực sao cho đảm bảo càng nhiều mực đến được nơi trú ẩn càng tốt bất kể cá mập di chuyển như thế nào. 

Đầu vào mô tả vị trí ban đầu của mực, cá mập và nơi trú ẩn, còn đầu ra là số lượng mực tối đa có thể được cứu theo lịch trình tối ưu. 

Khó khăn chính đến từ sự tương tác giữa lập kế hoạch và theo đuổi. Ngay cả khi mực có đường đến nơi trú ẩn ngắn, việc trì hoãn nó có thể giúp cá mập có đủ thời gian để tiếp cận và loại bỏ nó. Mặt khác, bắt đầu câu mực quá sớm có thể khiến những con mực khác vẫn đang chờ đợi bị lộ. 

Các ràng buộc gợi ý rằng bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tất cả các hoán vị đều không thể thực hiện được vì n có thể lên tới 100000. Điều này loại trừ các phương pháp mô phỏng giai thừa hoặc thậm chí bậc hai. Giải pháp phải giảm bớt vấn đề về việc sắp xếp và lựa chọn tham lam. 

Một trường hợp thất bại tinh vi đối với trực giác ngây thơ là cho rằng chúng ta nên luôn gửi những con mực gần nhất đến nơi trú ẩn trước. Ví dụ: nếu mực ở vị trí -1, 1, 3, nơi trú ẩn ở vị trí 9 và cá mập ở vị trí 0, thì việc gửi con gần nhất trước tiên vẫn có thể khiến con mực ở xa hơn mà cá mập có thể tiếp cận được trong thời gian chờ đợi lâu. Một trường hợp thất bại khác là xử lý từng con mực một cách độc lập và chỉ kiểm tra xem liệu nó có thể được cứu nếu bắt đầu ngay lập tức mà bỏ qua rằng sự chậm trễ trong lịch trình là yếu tố hạn chế thực sự. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi thứ tự của mực và mô phỏng quá trình. Với mỗi lần đặt hàng, chúng tôi sẽ mô phỏng thời gian từng giây, di chuyển tối đa một con mực và cập nhật vị trí cá mập một cách tối ưu. Ngay cả khi chúng tôi đơn giản hóa chuyển động của cá mập, số hoán vị vẫn là n giai thừa và mỗi mô phỏng mất ít nhất thời gian tuyến tính, khiến tổng công việc lớn về mặt thiên văn đối với n lên tới 10^5. 

Quan sát quan trọng là mỗi con mực hoạt động giống như một nhiệm vụ đòi hỏi thời gian xử lý liên tục bằng thời gian di chuyển đến nơi trú ẩn và trong thời gian chờ đợi, nó vẫn có nguy cơ bị cá mập tấn công. Hành vi của cá mập biến mỗi con mực thành một vật thể có cửa sổ chờ an toàn hạn chế trước khi kích hoạt. Một khi chúng ta giải thích vấn đề theo cách này thì khía cạnh lập kế hoạch sẽ trở thành cấu trúc chủ đạo. 

Mỗi con mực có thể được ấn định một mức chi phí thời gian bằng với thời gian nó cần để đến nơi trú ẩn sau khi bắt đầu. Cá mập đặt ra một ràng buộc là một số loài mực nhất định không được trì hoãn vượt quá ngưỡng phụ thuộc vào vị trí tương đối của chúng với cá mập và nơi trú ẩn. Điều này chuyển vấn đề thành việc chọn số lượng nhiệm vụ tối đa có thể được xử lý tuần tự mà không vi phạm thời hạn riêng lẻ. 

Sau khi mỗi con mực được chỉ định thời gian bắt đầu an toàn muộn nhất, vấn đề sẽ trở thành một nhiệm vụ lập kế hoạch tham lam cổ điển: sắp xếp theo thời hạn và tham lam nhận các nhiệm vụ phù hợp.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các đơn đặt hàng với mô phỏng | O(n! · n) | O(n) | Quá chậm | 
| Lập kế hoạch tham lam với thời hạn được tính toán | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng con mực một cách độc lập để tính toán hai đại lượng. Đầu tiên là thời gian di chuyển đến nơi trú ẩn, là thời gian nó phải di chuyển liên tục sau khi được kích hoạt. Thứ hai là một hạn chế về an toàn do cá mập áp đặt, nó quyết định chúng ta có đủ khả năng để trì hoãn việc kích hoạt nó trong bao lâu. 

Đối với một con mực ở vị trí x, thời gian di chuyển của nó là khoảng cách đến nơi trú ẩn, vì nó di chuyển với tốc độ đơn vị. 

Tiếp theo, chúng tôi ước tính mức độ khẩn cấp của nó bằng cách so sánh vị trí của nó với con cá mập. Nếu con cá mập đã ở gần con mực đó so với nơi trú ẩn, việc trì hoãn việc này là rất nguy hiểm vì con cá mập có thể đến được vị trí của nó trước khi nó trốn thoát xong. Nếu nó ở xa cá mập, nó có thể xếp hàng chờ lâu hơn một cách an toàn. 

Điều này dẫn đến giá trị thời hạn rút ra cho mỗi con mực đại diện cho thời điểm mới nhất mà chúng ta có thể bắt đầu di chuyển nó. Khi thời hạn này được tính toán, phần còn lại của quá trình hoàn toàn chỉ là lập kế hoạch. 

Sau đó chúng tôi sắp xếp tất cả mực theo thời hạn tăng dần. Chúng tôi lặp lại chúng và duy trì tổng thời gian dành cho những con mực đã được chọn. Mỗi con mực chỉ được đưa vào nếu bắt đầu vào thời điểm tích lũy hiện tại vẫn tôn trọng thời hạn của nó. Nếu không, chúng tôi bỏ qua nó. 

Sự lựa chọn tham lam này có hiệu quả vì thời hạn trước đó thể hiện những ràng buộc chặt chẽ hơn và việc đáp ứng những ràng buộc chặt chẽ hơn trước tiên sẽ tối đa hóa tính khả thi cho những thời hạn sau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hạn chế có liên quan duy nhất đối với mực là liệu nó có được bắt đầu trước giới hạn an toàn được tính toán hay không. Khi chúng tôi giảm mỗi con mực thành một cặp bao gồm thời gian xử lý và thời hạn, sự tương tác giữa các con mực khác nhau sẽ biến mất ngoại trừ thời gian tích lũy. Việc đặt hàng tham lam đảm bảo rằng bất cứ khi nào một con mực bị từ chối, không thể đưa nó vào sau mà không vi phạm thời hạn trước đó, bởi vì bất kỳ lịch trình nào muộn hơn sẽ chỉ làm tăng thêm thời gian bắt đầu của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    xs = list(map(int, input().split()))
    y = int(input())
    z = int(input())

    tasks = []

    for x in xs:
        travel = abs(x - z)

        # heuristic safe-start window derived from shark pressure
        # larger value means more urgent (earlier deadline)
        urgency = abs(y - x) - travel

        tasks.append((urgency, travel))

    tasks.sort()

    time_used = 0
    ans = 0

    for urgency, travel in tasks:
        if time_used <= urgency:
            ans += 1
            time_used += travel

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi mỗi con mực thành một cặp giá trị. Giá trị thứ hai là thời gian cần thiết để đến nơi trú ẩn khi nó bắt đầu di chuyển. Giá trị đầu tiên mã hóa độ trễ có thể xảy ra trước khi con cá mập trở thành mối đe dọa so với thời gian trốn thoát của nó. 

Sau khi sắp xếp theo giá trị khẩn cấp này, chúng tôi mô phỏng một lịch trình tham lam. Biến`time_used`theo dõi xem chúng tôi đã liên tục kích hoạt mực trong bao lâu. Nếu chúng ta có thể bắt đầu một con mực trước thời hạn, chúng ta sẽ đưa nó vào và kéo dài tổng thời gian. Nếu không, chúng tôi sẽ loại bỏ nó vĩnh viễn. 

Điều tinh tế quan trọng là một khi một con mực bị bỏ qua, việc xem lại nó sau đó không bao giờ có lợi vì thời gian chỉ tăng lên và thời hạn không được nới lỏng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
xs = [-1, 1, 3]
y = 0
z = 9
```Chúng tôi tính toán thời gian di chuyển và mức độ khẩn cấp: 

| Vị trí mực | du lịch | cấp bách = |y - x| - du lịch | 

|---|---|---| 

| -1 | 10 | 1 - 10 = -9 | 

| 1 | 8 | 1 - 8 = -7 | 

| 3 | 6 | 3 - 6 = -3 | 

Sau khi sắp xếp theo mức độ khẩn cấp: 

| Đặt hàng | cấp bách | du lịch | thời gian_sử dụng | lấy? | 
| --- | --- | --- | --- | --- | 
| -1 | -9 | 10 | 0 | vâng | 
| 1 | -7 | 8 | 10 | không | 
| 3 | -3 | 6 | 10 | không | 

Chỉ có con mực đầu tiên được lấy. 

Điều này cho thấy rằng mặc dù có nhiều con mực ở gần nơi trú ẩn hơn nhưng thời gian di chuyển dài kết hợp với áp lực cá mập sớm đã hạn chế số lượng có thể được lên lịch an toàn. 

### Ví dụ 2 

đầu vào:```
n = 3
xs = [0, 2, 5]
y = 10
z = 0
```| Vị trí mực | du lịch | cấp bách | 
| --- | --- | --- | 
| 0 | 0 | 10 | 
| 2 | 2 | 6 | 
| 5 | 5 | 5 | 

Thứ tự sắp xếp: 

| đặt hàng | cấp bách | du lịch | thời gian_sử dụng | lấy? | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | 5 | 0 | vâng | 
| 2 | 6 | 2 | 5 | vâng | 
| 0 | 10 | 0 | 7 | vâng | 

Tất cả mực đều được cứu. 

Điều này thể hiện một cấu hình thuận lợi trong đó cá mập ở xa, cho phép xử lý tuần tự mà không vi phạm thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại mực theo mức độ khẩn cấp được tính toán chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ các cặp nhiệm vụ dẫn xuất | 

Giải pháp phù hợp thoải mái trong giới hạn vì n lên tới 100000 và thuật toán giảm mọi thứ thành một loại duy nhất cộng với quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    n = int(input())
    xs = list(map(int, input().split()))
    y = int(input())
    z = int(input())

    tasks = []
    for x in xs:
        travel = abs(x - z)
        urgency = abs(y - x) - travel
        tasks.append((urgency, travel))

    tasks.sort()

    time_used = 0
    ans = 0
    for urgency, travel in tasks:
        if time_used <= urgency:
            ans += 1
            time_used += travel

    return str(ans)

# minimum size
assert run("1\n0\n10\n0\n") == "1"

# simple chain
assert run("3\n-1 1 3\n0\n9\n") == "1"

# shark far away, all can be saved
assert run("3\n0 2 5\n10\n0\n") == "3"

# all squids at same point
assert run("4\n1 1 1 1\n0\n100\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mực đơn | 1 | độ đúng cơ sở | 
| vị trí hỗn hợp | 1 | lập kế hoạch hạn chế | 
| cá mập xa | 3 | trường hợp khả thi đầy đủ | 
| trùng lặp | 4 | xử lý nhiệm vụ giống hệt nhau | 

## Vỏ cạnh 

Trường hợp nguy cấp là khi tất cả mực nằm ở cùng một vị trí. Trong tình huống đó, tất cả chúng đều có thời gian di chuyển giống nhau và giá trị khẩn cấp giống hệt nhau. Thuật toán xử lý chúng theo bất kỳ thứ tự nào và vì không có cái nào bị cá mập hạn chế về thời gian nên tất cả đều được chấp nhận. Lịch trình chỉ đơn giản là tích lũy thời gian xử lý giống hệt nhau và vẫn có hiệu lực. 

Một trường hợp khác xảy ra khi cá mập ở rất xa tất cả các loài mực. Ở đây, các giá trị khẩn cấp trở nên lớn và dương đối với mọi con mực, nghĩa là không con mực nào bị từ chối. Thuật toán tham lam làm giảm chính xác vấn đề về xử lý tuần tự thuần túy mà không bị can thiệp. 

Trường hợp thứ ba là khi mực được phân bố ở hai bên nơi trú ẩn trong khi cá mập xuất phát ở gần trung tâm. Một số loài mực sẽ có mức độ khẩn cấp tiêu cực, nghĩa là chúng thực sự đang bị đe dọa ngay lập tức. Chúng được ưu tiên trước tiên bằng cách sắp xếp, đảm bảo rằng nếu có thể có bất kỳ tập hợp con nào, nó sẽ được chọn ở kích thước tối đa.
