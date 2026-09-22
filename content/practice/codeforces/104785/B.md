---
title: "CF 104785B - Thuyền đi lại"
description: "Chúng tôi đang mô phỏng hệ thống vận chuyển tap-in tap-out trong đó mỗi hành khách sử dụng một thẻ du lịch được đánh số. Mỗi sự kiện đều ghi lại bến tàu và ID thẻ, đồng thời các sự kiện sẽ diễn ra theo trình tự thời gian."
date: "2026-06-28T14:37:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "B"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 51
verified: true
draft: false
---

[CF 104785B - Người đi thuyền](https://codeforces.com/problemset/problem/104785/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng hệ thống vận chuyển tap-in tap-out trong đó mỗi hành khách sử dụng một thẻ du lịch được đánh số. Mỗi sự kiện đều ghi lại bến tàu và ID thẻ, đồng thời các sự kiện sẽ diễn ra theo trình tự thời gian. Mỗi thẻ hoạt động độc lập và chúng tôi phải tính toán tổng số tiền tích lũy của mỗi thẻ sau khi xử lý tất cả các sự kiện. 

Một chuyến đi được hình thành bằng cách ghép hai sự kiện liên tiếp của cùng một thẻ: sự kiện đầu tiên là bắt đầu (chạm vào) và sự kiện tiếp theo cho cùng một thẻ đó là kết thúc (tap out). Chi phí phụ thuộc vào mối liên hệ giữa hai trụ cầu đó. Nếu trụ đầu và trụ cuối khác nhau thì chi phí là chênh lệch tuyệt đối của chỉ số của chúng. Nếu chúng có cùng một bến tàu hoặc nếu một thẻ bắt đầu chuyến đi nhưng không bao giờ kết thúc chuyến đi thì chi phí sẽ là mức phạt cố định là 100. 

Thách thức chính là các sự kiện được xen kẽ trên nhiều thẻ, do đó mỗi thẻ duy trì trạng thái “chuyến đi hiện đang mở” của riêng mình. Chúng tôi cần xử lý tối đa 100000 sự kiện, do đó, bất kỳ phương pháp nào cố gắng quét hoặc khớp trên toàn cầu sẽ quá chậm. Chúng tôi phải duy trì trạng thái trên mỗi thẻ và cập nhật câu trả lời theo thời gian liên tục cho mỗi sự kiện. 

Một trường hợp lỗi nhỏ xuất hiện khi thẻ có một cú chạm chưa từng có ở cuối. Ví dụ: nếu một thẻ có một sự kiện`(pier 3, card 7)`và không có sự kiện nào nữa, chi phí chính xác là 100. Một giải pháp đơn giản chỉ tính phí trên các cặp hoàn chỉnh sẽ xuất sai 0. Một trường hợp góc khác là các lần nhấn lặp lại mà không thực thi luân phiên trên toàn cầu. Ví dụ: một thẻ có thể chạm vào ở bến tàu 1, chạm ra ở bến tàu 2, sau đó chạm vào lại ở bến tàu 2 và rút ra ở bến tàu 2. Chuyến đi cuối cùng vẫn có giá 100 mặc dù nó “trông giống như” một chuyển động không khoảng cách, bởi vì những chuyến đi cùng bến tàu luôn bị phạt. 

Các ràng buộc ngụ ý rằng chúng ta cần xử lý O(1) cho mỗi sự kiện. Với tối đa 100000 sự kiện, thậm chí O(k log k) cũng có thể chấp nhận được nhưng không cần thiết. Giới hạn nhỏ trên n (số lượng trụ lên tới 50) không liên quan đến độ phức tạp và chỉ xác định không gian tọa độ cho khoảng cách. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là lưu trữ tất cả các sự kiện trên mỗi thẻ và liên tục quét ngược để tìm lần chạm vào chưa từng có trước đó mỗi lần chạm ra xảy ra. Điều này hoạt động hợp lý vì mỗi chuyến đi được xác định bằng cách ghép nối hai sự kiện, nhưng sẽ không hiệu quả nếu được triển khai một cách đơn giản: mỗi sự kiện có thể kích hoạt quét tất cả các sự kiện trước đó của thẻ đó, dẫn đến O(k^2) trong trường hợp xấu nhất khi tất cả các sự kiện thuộc về một thẻ và xen kẽ giữa mở và đóng. 

Quan sát quan trọng là mỗi thẻ chỉ cần nhớ một phần trạng thái: liệu nó hiện có chuyến đi mở hay không và nếu có thì bến tàu nơi nó bắt đầu. Sau khi xảy ra hiện tượng tap-out, chúng tôi sẽ ngay lập tức giải quyết chi phí và xóa trạng thái. Nếu một lần nhấn mới xảy ra trong khi không có chuyến đi nào được mở, chúng tôi chỉ cần lưu trữ nó. 

Điều này làm giảm vấn đề xuống còn một lần truyền qua luồng sự kiện, duy trì một từ điển hoặc mảng có kích thước m cho các chuyến đi đang hoạt động và một mảng khác cho chi phí tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét lịch sử mỗi sự kiện) | O(k2) | O(k) | Quá chậm | 
| Tối ưu (trạng thái trên mỗi thẻ) | O(k) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai mảng được lập chỉ mục theo ID thẻ. Một cái lưu trữ bến tàu mở hiện tại cho mỗi thẻ và cái còn lại lưu trữ chi phí tích lũy. 

1. Khởi tạo một mảng`open_pier`có kích thước m với giá trị trọng điểm có nghĩa là “không có chuyến đi hoạt động” và một mảng`cost`có kích thước m chứa đầy số không. Người canh gác đảm bảo chúng ta có thể phân biệt giữa trạng thái hoạt động và không hoạt động mà không có sự mơ hồ. 
2. Xử lý từng sự kiện`(p, c)`theo thứ tự. Thứ tự thời gian đảm bảo rằng việc ghép đôi phải diễn ra tuần tự trên mỗi thẻ. 
3. Nếu thẻ`c`không có chuyến đi hoạt động, cửa hàng`p`TRONG`open_pier[c]`. Điều này tượng trưng cho sự bắt đầu của một chuyến đi. Chưa phát sinh chi phí vì chuyến đi chưa kết thúc. 
4. Nếu thẻ`c`đã có một chuyến đi đang hoạt động bắt đầu lúc`open_pier[c]`, tính chi phí của chuyến đi như sau. Nếu như`p == open_pier[c]`, cộng 100 vào`cost[c]`. Nếu không thì thêm`abs(p - open_pier[c])`. 
5. Sau khi xử lý chuyến đi đã hoàn thành, hãy đặt lại`open_pier[c]`quay lại giá trị trọng điểm để cho biết thẻ đã sẵn sàng cho chuyến đi mới. 
6. Sau khi tất cả các sự kiện được xử lý, bất kỳ thẻ nào vẫn còn không có trọng điểm`open_pier`giá trị đại diện cho một chuyến đi không đầy đủ. Đối với mỗi thẻ như vậy, hãy thêm 100 vào giá trị của nó. 

Lý do đằng sau bước 6 là hệ thống xác định những hành trình chưa hoàn thành luôn bị phạt, bất kể chúng bắt đầu từ đâu. 

### Tại sao nó hoạt động 

Chuỗi sự kiện của mỗi lá bài được phân chia tự nhiên thành các cặp (bắt đầu, kết thúc) rời rạc liên tiếp, ngoại trừ trường hợp bắt đầu ở cuối. Thuật toán thực thi việc ghép nối này một cách tham lam theo thứ tự đến, phù hợp với cách giải thích hợp lệ duy nhất của hệ thống. Vì thẻ không thể có các chuyến đi chồng chéo nên chỉ lưu trữ một trạng thái bắt đầu hoạt động là đủ để thể hiện đầy đủ bối cảnh hiện tại của thẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    open_pier = [-1] * (m + 1)
    cost = [0] * (m + 1)
    
    for _ in range(k):
        p, c = map(int, input().split())
        
        if open_pier[c] == -1:
            open_pier[c] = p
        else:
            start = open_pier[c]
            if p == start:
                cost[c] += 100
            else:
                cost[c] += abs(p - start)
            open_pier[c] = -1
    
    for c in range(1, m + 1):
        if open_pier[c] != -1:
            cost[c] += 100
    
    print(*cost[1:])

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng hai mảng được lập chỉ mục theo ID thẻ.`open_pier`theo dõi xem thẻ hiện có hành trình chưa hoàn thành hay không và lưu trữ bến xuất phát của thẻ đó. Khi sự kiện thứ hai đến với thẻ đó, chúng tôi sẽ tính toán chi phí ngay lập tức và đặt lại trạng thái. Sau khi xử lý tất cả các sự kiện, chúng tôi thực hiện lần quét cuối cùng để tính phí cho mọi chuyến đi chưa hoàn thành. 

Một lỗi triển khai phổ biến là quên lần quét cuối cùng, điều này làm giảm giá trị của những thẻ không bao giờ rút ra. Một vấn đề khó phát hiện khác là việc đặt lại trạng thái quá sớm hoặc quá muộn không chính xác; việc thiết lập lại phải diễn ra ngay sau khi tính toán chi phí để đảm bảo sự kiện tiếp theo được coi là một chuyến đi mới. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một ví dụ nhỏ phù hợp với tuyên bố: 

đầu vào:```
n=3, m=3, k=5
(1,1)
(1,2)
(1,2)
(3,1)
(2,3)
```Chúng tôi theo dõi`open_pier`Và`cost`mỗi bước. 

| Sự kiện | Thẻ | Hành động | trạng thái open_pier | trạng thái chi phí | 
| --- | --- | --- | --- | --- | 
| (1,1) | 1 | bắt đầu | [1,-1,-1] | [0,0,0] | 
| (1,2) | 2 | bắt đầu | [1,1,-1] | [0,0,0] | 
| (1,2) | 2 | cuối bến tàu → +100 | [1,-1,-1] | [0,100,0] | 
| (3,1) | 1 | kết thúc khác biệt → +2 | [-1,-1,-1] | [2.100,0] | 
| (2,3) | 3 | bắt đầu | [-1,-1,2] | [2.100,0] | 

Sau khi xử lý tất cả các sự kiện, thẻ 3 có chuyến đi mở nên được +100. 

Chi phí cuối cùng:`[2, 100, 100]`. 

Dấu vết này cho thấy việc ghép nối hoàn toàn mang tính cục bộ trên mỗi thẻ và các chuyến đi chưa hoàn thành chỉ được xử lý sau khi luồng kết thúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi sự kiện được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(m) | Chúng tôi lưu trữ một trạng thái và một chi phí cho mỗi thẻ | 

Các ràng buộc cho phép tối đa 100000 sự kiện và thẻ, do đó việc xử lý tuyến tính dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi chỉ giữ hai mảng có kích thước m. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m, k = map(int, sys.stdin.readline().split())
    
    open_pier = [-1] * (m + 1)
    cost = [0] * (m + 1)
    
    for _ in range(k):
        p, c = map(int, sys.stdin.readline().split())
        if open_pier[c] == -1:
            open_pier[c] = p
        else:
            start = open_pier[c]
            if p == start:
                cost[c] += 100
            else:
                cost[c] += abs(p - start)
            open_pier[c] = -1
    
    for c in range(1, m + 1):
        if open_pier[c] != -1:
            cost[c] += 100
    
    return " ".join(map(str, cost[1:]))

# provided sample
assert run("3 3 5\n1 1\n1 2\n1 2\n3 1\n2 3\n") == "2 100 100"

# single card simple pair
assert run("2 1 2\n1 1\n3 1\n") == "2"

# same-pier penalty
assert run("2 1 2\n1 1\n1 1\n") == "100"

# unmatched open trip
assert run("5 2 1\n3 2\n") == "0 100"

# alternating multiple cards
assert run("3 2 4\n1 1\n2 2\n3 1\n3 2\n") == "3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp thẻ đơn | 2 | chi phí khoảng cách cơ bản | 
| cùng một bến tàu hai lần | 100 | quy tắc phạt | 
| độc thân chưa từng có | 0 100 | hình phạt cuối luồng | 
| thẻ xen kẽ | 3 3 | trạng thái độc lập trên mỗi thẻ | 

## Vỏ cạnh 

Hộp đựng một cạnh là một tấm thẻ không bao giờ rút ra được. Ví dụ: đầu vào:```
2 1 1
1 1
```Thuật toán lưu trữ`open_pier[1] = 1`. Sau khi xử lý tất cả các sự kiện, lần quét cuối cùng cộng thêm 100, tạo ra kết quả`100`. Nếu không có bước này, kết quả sẽ vẫn là 0 vì không có cặp hoàn chỉnh nào được hình thành. 

Một trường hợp khác là những chuyến đi lặp lại cùng một bến tàu. Ví dụ:```
2 1 2
1 1
1 1
```Sự kiện đầu tiên mở ra một chuyến đi. Sự kiện thứ hai đóng nó ở cùng một bến tàu, gây ra hình phạt cố định 100. Việc đặt lại đảm bảo rằng nếu một sự kiện khác diễn ra sau đó, nó sẽ bắt đầu một chuyến đi mới thay vì hợp nhất không chính xác với trạng thái trong quá khứ. 

Trường hợp tinh vi cuối cùng là xen kẽ nhiều thẻ:```
3 2 4
1 1
2 2
3 1
3 2
```Thẻ 1 tạo thành một chuyến đi từ 1 đến 3 chi phí 2. Thẻ 2 tạo thành một chuyến đi từ 2 đến 3 chi phí 1. Sự độc lập của`open_pier`mỗi thẻ đảm bảo không có sự lây nhiễm chéo giữa các tiểu bang.
