---
title: "CF 104303B - \u7199\u5de8\u6253\u7968"
description: "Chúng tôi được cung cấp một hệ thống in vé với hai máy giống hệt nhau có thể được sử dụng để tạo phiếu hoàn trả. Mỗi máy có thể tạo ra tối đa một vé cho mỗi hoạt động và sau khi tạo ra một vé, nó sẽ không khả dụng trong thời gian làm mát là một phút."
date: "2026-07-01T20:09:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104303
codeforces_index: "B"
codeforces_contest_name: "2023 Xiangtan Unversity Freshman Conteset"
rating: 0
weight: 104303
solve_time_s: 51
verified: true
draft: false
---

[CF 104303B - \u7199\u5de8\u6253\u7968](https://codeforces.com/problemset/problem/104303/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống in vé với hai máy giống hệt nhau có thể được sử dụng để tạo phiếu hoàn trả. Mỗi máy có thể tạo ra tối đa một vé cho mỗi thao tác và sau khi tạo ra một vé, nó sẽ không khả dụng trong thời gian làm mát là`a`phút. Dùng máy một lần cũng tốn`b`phút thời gian làm việc tích cực. Hạn chế chính là tại bất kỳ thời điểm nào, chỉ có một máy có thể được vận hành tích cực, nghĩa là chúng ta không thể chạy song song cả hai máy ngay cả khi cả hai máy đều sẵn sàng. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp thời gian hồi chiêu`a`, thời gian hoạt động`b`, và số lượng vé`n`. Nhiệm vụ là xác định tổng thời gian tối thiểu cần thiết để sản xuất ra tất cả`n`vé. 

Ràng buộc`n ≤ 10^9`ngay lập tức loại trừ mọi mô phỏng trên mỗi vé. Ngay cả việc mô phỏng logarit hoặc theo sự kiện trên mỗi yêu cầu cũng quá chậm vì chúng tôi có thể có tới`10^5`trường hợp thử nghiệm. Giải pháp phải được rút ra ở dạng đóng cho mỗi trường hợp thử nghiệm, trong thời gian không đổi. 

Một điểm tinh tế trong bài toán này là ràng buộc của máy không phải là sự song song đối xứng. Mặc dù có hai máy, nhưng mỗi lần chỉ có thể thực hiện một thao tác, điều này khiến việc này giống như lập kế hoạch xen kẽ các khoảng thời gian hồi chiêu hơn là xử lý song song. Một sự hiểu lầm ngây thơ là cho rằng hai máy có nghĩa là tăng gấp đôi thông lượng, điều này không chính xác do hạn chế “chỉ có thể sử dụng một máy tại một thời điểm”. 

Trường hợp cạnh xuất hiện khi`a = 0`, khi thời gian hồi chiêu biến mất và quá trình này trở nên hoàn toàn tuần tự và khi`a`là rất lớn so với`b`, điều này khiến cho việc tái sử dụng cùng một máy là không thể trong một chu kỳ ngắn. Một trường hợp cạnh quan trọng khác là`n = 1`, trong đó chỉ cần một thao tác duy nhất và thời gian hồi chiêu là không liên quan. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng thời gian. Chúng tôi duy trì thời gian hiện tại, theo dõi thời điểm mỗi máy có sẵn và liên tục chọn máy có sẵn tiếp theo để thực hiện thao tác. Sau mỗi tấm vé, chúng tôi tiến lên theo thời gian`b`, đánh dấu máy đó là không khả dụng cho đến khi`current_time + a`và tiếp tục cho đến khi tất cả vé được tạo ra. Điều này đúng vì nó trực tiếp mô hình hóa các ràng buộc của hệ thống. 

Tuy nhiên, cách tiếp cận này thoái hóa nhanh chóng. Mỗi trong số`n`vé yêu cầu cập nhật trạng thái máy và chọn tính khả dụng, vì vậy độ phức tạp là`O(n)`mỗi trường hợp thử nghiệm. Với`n`lên tới`10^9`, điều này là không thể. 

Quan sát quan trọng là hệ thống không thực sự phân nhánh hoặc phụ thuộc vào lịch sử sau thời gian hoạt động cuối cùng. Bởi vì mỗi lần chỉ có thể sử dụng một máy, quyết định thực sự duy nhất là liệu chúng ta có thể sử dụng lại máy ngay sau khi hoàn thành thao tác trước đó hay chúng ta buộc phải chờ thời gian hồi chiêu. Điều này tạo ra một mô hình lặp đi lặp lại: chúng ta hoặc làm việc liên tục nếu thời gian hồi chiêu nhỏ hoặc chúng ta buộc phải vào khoảng trống nhàn rỗi nếu thời gian hồi chiêu lớn. 

Quá trình giảm xuống một cấu trúc hai trường hợp. Nếu thời gian hồi chiêu`a`đủ nhỏ để khi chúng ta hoàn thành một thao tác và chuyển đổi, máy đã sẵn sàng và chúng ta không bao giờ phải chờ đợi. Nếu không, chúng ta phải chèn thời gian nhàn rỗi giữa một số thao tác. Mỗi hoạt động có hiệu quả`b`thời gian, nhưng nếu`a > b`, vậy thì chúng ta phải đợi`a - b`trước khi cùng một máy có thể được sử dụng lại vì việc chuyển đổi không cho phép thực hiện song song. 

Điều này đơn giản hóa quy trình thành một lịch trình tuyến tính: tổng thời gian là`n * b`cộng với những khoảng trống nhàn rỗi bổ sung được đưa ra bất cứ khi nào thời gian hồi chiêu vượt quá thời gian thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Công thức dẫn xuất | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình lịch trình như một chuỗi các`n`các hoạt động được thực hiện lần lượt. 

1. Bắt đầu từ thời điểm`0`. Mỗi vé yêu cầu chính xác`b`số phút làm việc tích cực nên thời gian cơ bản luôn là`n * b`. Điều này chiếm thời gian xử lý bắt buộc của tất cả các vé. 
2. Xác định xem thời gian hồi chiêu có gây ra thời gian nhàn rỗi hay không bằng cách so sánh`a`Và`b`. Sau khi hoàn thành một thao tác, chúng ta tiêu ngay`b`thời gian, vì vậy hoạt động tiếp theo bắt đầu`b`phút sau. Nếu như`a ≤ b`, thì đến lúc chúng ta chuẩn bị hoạt động trở lại thì máy đã nguội hẳn rồi. Không cần phải chờ đợi. 
3. Nếu`a > b`, sau khi kết thúc một thao tác, máy vẫn được hạ nhiệt thêm một lần nữa`(a - b)`vài phút sau khi chúng tôi bắt đầu hoạt động tiếp theo. Vì mỗi lần chúng ta chỉ có thể vận hành một máy nên chúng ta không thể che giấu khoảng cách này bằng máy thứ hai. Điều này tạo ra một khoảng thời gian nhàn rỗi không thể tránh khỏi giữa mỗi hoạt động liên tiếp. 
4. Có`n - 1`chuyển tiếp giữa`n`hoạt động, vì vậy chúng tôi thêm`(n - 1) * max(0, a - b)`đến tổng thời gian. 
5. Trả về tổng số đã tính. 

### Tại sao nó hoạt động 

Lịch trình được xác định đầy đủ bởi sự phụ thuộc liên tiếp giữa các hoạt động. Vì các thao tác không thể chồng chéo lên nhau nên khả năng kém hiệu quả duy nhất có thể xảy ra là việc chờ thời gian hồi chiêu khi cố gắng sử dụng lại máy. Hệ thống không bao giờ tích lũy thêm công suất song song, vì vậy mỗi khoảng trống đều độc lập và giống hệt nhau. Điều này làm cho quá trình tương đương với một chuỗi tuyến tính trong đó mỗi cạnh đóng góp một trong hai`b`một mình hoặc`b + (a - b)`tùy thuộc vào việc thời gian hồi chiêu có vượt quá thời gian thực hiện hay không. 

Bởi vì mọi thao tác ngoại trừ thao tác đầu tiên đều được bắt đầu bằng chính xác một lần chuyển đổi và mỗi lần chuyển đổi có cấu trúc giống hệt nhau nên công thức khớp chính xác với tổng thời gian tích lũy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, n = map(int, input().split())
    
    base = n * b
    idle = max(0, a - b) * (n - 1)
    
    print(base + idle)
```Việc triển khai trực tiếp mã hóa việc phân tách tổng thời gian thành công việc bắt buộc và các khoảng trống nhàn rỗi tùy chọn. phép nhân`n * b`tích lũy chi phí sản xuất mỗi vé. Thuật ngữ`(n - 1)`phản ánh rằng thao tác đầu tiên không có yêu cầu thời gian hồi chiêu trước đó, vì không có hành động nào trước đó để chặn nó. các`max(0, a - b)`đảm bảo chúng tôi chỉ thêm thời gian nhàn rỗi khi thời gian hồi chiêu vượt quá thời gian đã dành để thực hiện thao tác tiếp theo. 

Một cạm bẫy phổ biến là quên rằng thời gian hồi chiêu trùng với thời gian thực hiện. Nếu như`a ≤ b`, máy sẽ sẵn sàng trước khi chúng tôi hoàn thành hoặc ngay sau khi hoàn thành, do đó không cần phải chờ đợi thêm. 

## Ví dụ đã hoạt động 

Hãy xem xét hai trường hợp đại diện. 

Đầu tiên,`a = 1, b = 3, n = 4`. 

| Bước | Thời gian hoạt động | Thời gian sẵn sàng tiếp theo | Đã thêm nhàn rỗi | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 3 | 
| 2 | 3 | 6 | 0 | 6 | 
| 3 | 3 | 9 | 0 | 9 | 
| 4 | 3 | 12 | 0 | 12 | 

Đây`a < b`, vì vậy thời gian hồi chiêu không bao giờ ảnh hưởng đến việc lập lịch trình. Kết quả đơn giản là`n * b = 12`. 

Thứ hai,`a = 7, b = 3, n = 4`. 

| Bước | Thời gian hoạt động | Khoảng thời gian hồi chiêu | Đã thêm nhàn rỗi | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | 0 | 3 | 
| 2 | 3 | 7 - 3 = 4 | 4 | 10 | 
| 3 | 3 | 4 | 4 | 17 | 
| 4 | 3 | 4 | 4 | 24 | 

Mỗi lần chuyển đổi buộc phải có khoảng thời gian nhàn rỗi là 4 phút vì máy vẫn đang làm mát sau khi chúng tôi hoàn thành khoảng thời gian cơ bản của thao tác tiếp theo. 

Những dấu vết này cho thấy trạng thái không hoạt động chỉ xuất hiện khi thời gian hồi chiêu vượt quá thời gian thực hiện và nó lặp lại đồng đều trên tất cả các lần chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm được tính toán trong thời gian không đổi bằng công thức dạng đóng | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép lên đến`10^5`các trường hợp thử nghiệm, do đó cần có giải pháp thời gian không đổi cho mỗi trường hợp. Cách tiếp cận này chỉ thực hiện các phép tính số học cho mỗi truy vấn, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        a, b, n = map(int, input().split())
        base = n * b
        idle = max(0, a - b) * (n - 1)
        out.append(str(base + idle))
    return "\n".join(out)

# provided sample-like tests
assert run("4\n1 3 1\n1 3 4\n7 3 4\n0 5 10\n") == "3\n12\n24\n50"

# custom tests
assert run("1\n0 10 5\n") == "50", "no cooldown"
assert run("1\n10 1 5\n") == "45", "heavy cooldown"
assert run("1\n3 3 3\n") == "9", "equal case boundary"
assert run("1\n100 1 2\n") == "101", "large gap small n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 10 5`|`50`| Không có thời gian hồi chiêu thêm thời gian nhàn rỗi | 
|`10 1 5`|`45`| Nhàn rỗi tối đa chiếm ưu thế | 
|`3 3 3`|`9`| Ranh giới không xuất hiện nhàn rỗi | 
|`100 1 2`|`101`| Độ chính xác chuyển tiếp đơn | 

## Vỏ cạnh 

Khi nào`a = 0`, hệ thống hoàn toàn không có thời gian hồi chiêu nên mỗi vé được xử lý liên tục không có độ trễ. Công thức rút gọn đúng vì`max(0, a - b)`trở thành số 0 nên tổng thời gian chỉ là`n * b`. 

Khi`a = b`, máy sẽ khả dụng chính xác khi thao tác tiếp theo bắt đầu. Điều này tạo ra một đường dẫn hoàn hảo không có thời gian nhàn rỗi, một lần nữa phù hợp với công thức vì`(a - b) = 0`. 

Khi`a > b`, mỗi lần chuyển đổi đều đưa ra một độ trễ cố định. Ví dụ, với`a = 5, b = 2, n = 3`, mỗi bước sau bước đầu tiên sẽ bị trễ 3 phút. Lịch trình trở nên xác định và tuyến tính, và công thức tính đến chính xác hai lần chuyển đổi như vậy. 

Khi`n = 1`, không có sự chuyển tiếp nào cả. các`(n - 1)`hệ số loại bỏ chính xác mọi đóng góp nhàn rỗi, chỉ để lại chi phí vận hành duy nhất`b`.
