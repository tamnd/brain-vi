---
title: "CF 105622B - Trò chơi trên cây"
description: "Trò chơi được chơi trên một cái cây. Spyrosaliv bắt đầu trên nút s và muốn tiếp cận nút d. Ở mỗi lượt, anh ta phải di chuyển qua một rìa hiện không bị chặn. Đạt d ngay lập tức thắng. Nếu anh ta không có động thái hợp pháp, anh ta sẽ thua."
date: "2026-06-26T18:16:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105622
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #38 (Tree-Forces)"
rating: 0
weight: 105622
solve_time_s: 56
verified: true
draft: false
---

[CF 105622B - Trò chơi trên cây](https://codeforces.com/problemset/problem/105622/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một cái cây. Spyrosaliv bắt đầu trên nút`s`và muốn tiếp cận nút`d`. Ở mỗi lượt, anh ta phải di chuyển qua một rìa hiện không bị chặn. Tiếp cận`d`thắng ngay. Nếu anh ta không có động thái hợp pháp, anh ta sẽ thua. 

Sau mỗi bước di chuyển của Spyrosaliv, Cow the Cow sẽ đến lượt. Con bò điều khiển một cạnh bị chặn. Nó có thể loại bỏ khối trước đó và đặt khối mới vào một cạnh khác. Con bò đang cố gắng ngăn chặn Spyrosaliv tiếp cận`d`. 

Câu hỏi quan trọng là liệu vị trí xuất phát có cơ hội nào chống lại một đối thủ luôn có thể phản ứng sau khi nhìn thấy nước đi trước đó hay không. 

Số lượng nút có thể lớn tới mức`200000`, vì vậy bất kỳ giải pháp nào khám phá nhiều trạng thái trò chơi có thể xảy ra đều không thể thực hiện được. Cây có kích thước này chỉ cho phép các thuật toán gần với thời gian tuyến tính. Chúng ta cần tìm đặc tính cấu trúc của cây thay vì mô phỏng chuyển động. 

Các trường hợp có lợi thế không rõ ràng là do Spyrosaliv di chuyển trước. Nếu đích đến ở ngay gần đó, con bò sẽ không bao giờ có cơ hội can thiệp. 

Ví dụ:```
2 1 2
1 2
```Đầu ra đúng là:```
YES
```Spyrosaliv ngay lập tức di chuyển từ nút`1`đến nút`2`. 

Một cách tiếp cận bất cẩn chỉ xem xét liệu con bò có thể chặn đường hay không sẽ nói không chính xác rằng con bò luôn thắng vì nó bỏ qua lợi thế nước đi đầu tiên. 

Một trường hợp quan trọng khác là một con đường dài hơn:```
3 1 3
1 2
2 3
```Đầu ra đúng là:```
NO
```Spyrosaliv di chuyển đến nút`2`. Con bò chặn cạnh duy nhất dẫn đến nút`3`, nên Spyrosaliv không thể tiếp tục hướng tới mục tiêu. Việc lùi lại chỉ mang lại cho con bò một cơ hội khác để chặn rìa phía sau nó. 

Vấn đề tương tự cũng xuất hiện ở những cây lớn hơn. Các nhánh bổ sung không tạo ra chiến lược chiến thắng vì con bò luôn có thể chặn rìa đi về đích sau khi Spyrosaliv rời khỏi nút hiện tại của mình. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là coi trò chơi như một tìm kiếm biểu đồ. Một trạng thái sẽ chứa nút hiện tại của Spyrosaliv và cạnh hiện đang bị chặn. Từ mọi tiểu bang, chúng tôi có thể thử mọi động thái hợp pháp, sau đó mô phỏng mọi phản ứng có thể có của con bò. Điều này đúng vì nó mô hình chính xác cách chơi tối ưu, nhưng số lượng trạng thái quá lớn. 

Có tới`n`các vị trí có thể và`n - 1`các cạnh có thể bị chặn, đưa ra khoảng`O(n^2)`tiểu bang. Vì`n = 200000`, điều này vượt xa những gì có thể phù hợp với thời gian hoặc trí nhớ. 

Quan sát quan trọng đến từ việc lấy rễ cây tại nút đích`d`. Mọi nút ngoại trừ`d`có chính xác một cạnh cha di chuyển đến gần đích hơn. Nếu Spyrosaliv không liền kề với`d`, bước đi đầu tiên của anh ta có thể coi là chuyển sang một đứa con nào đó của`d`cây có rễ hoặc đang di chuyển lên phía trên`d`. Sau mỗi bước tiến thành công về phía`d`, con bò có thể chặn chính xác cạnh mà Spyrosaliv sẽ cần tiếp theo. 

Giả sử Spyrosaliv hiện đang ở nút`u`, và cạnh từ`u`tới cha mẹ của nó là hướng hữu ích duy nhất. Sau khi Spyrosaliv di chuyển đi đâu đó, con bò có thể chặn cạnh gốc của nút mới. Spyrosaliv sau đó bị buộc phải rời khỏi đích đến. Việc lặp lại chiến lược này sẽ đẩy Spyrosaliv đi xa hơn`d`cho đến khi anh ta đi vào ngõ cụt. 

Lần duy nhất con bò không có được cơ hội này là ở nước đi đầu tiên. Nếu như`s`liền kề với`d`, Spyrosaliv đến đích ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) trạng thái | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và lưu trữ các nút lân cận của mỗi nút. Thông tin duy nhất cần thiết là liệu`d`được kết nối trực tiếp với`s`, vì vậy việc duyệt toàn bộ là không cần thiết. 
2. Kiểm tra mọi hàng xóm của`s`. Nếu một trong số họ là`d`, Spyrosaliv có thể di chuyển đến đó trong lượt đầu tiên và giành chiến thắng trước khi con bò hành động. 
3. Nếu`d`không liền kề với`s`, đầu ra`NO`. Con bò luôn có thể chặn bước tiến của Spyrosaliv sau nước đi đầu tiên bằng cách chặn rìa dẫn về đích. 

Tại sao nó hoạt động: 

Rễ cây tại`d`. Mỗi nút có chính xác một cạnh làm giảm khoảng cách của nó tới`d`. Spyrosaliv cần phải sử dụng nhiều lần các cạnh như vậy để tiếp cận mục tiêu. Sau mỗi lần di chuyển, con bò biết vị trí mới của Spyrosaliv và có thể chặn cạnh giảm tiếp theo đó. Vì con bò hành động theo sau Spyrosaliv nên lối thoát duy nhất có thể thoát khỏi chiến lược này là tiếp cận`d`trong lần di chuyển đầu tiên. Điều đó xảy ra chính xác khi`s`Và`d`liền kề nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s, d = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    for v in adj[s]:
        if v == d:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ cần danh sách kề vì câu trả lời phụ thuộc vào bước đi đầu tiên. Chúng tôi không cần lưu trữ mối quan hệ cha mẹ hoặc mô phỏng trò chơi. 

Vòng lặp kết thúc`adj[s]`kiểm tra mọi bước đi đầu tiên có thể. Nếu đích đến xuất hiện ở đó, trò chơi sẽ kết thúc ngay lập tức theo hướng có lợi cho Spyrosaliv. Nếu không, chiến lược chặn của con bò sẽ hoạt động bất kể phần còn lại của cây. 

Không có thủ thuật lập chỉ mục nào liên quan vì các nút được đánh số từ`1`và danh sách kề được tạo với kích thước`n + 1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2 4
1 4
2 5
4 3
3 5
```Thông tin nút bắt đầu có liên quan là: 

| Nút hiện tại | Hàng xóm đã kiểm tra | Là đích đến? | 
| --- | --- | --- | 
| 2 | 5 | Không | 

Thuật toán không bao giờ tìm thấy nút`4`là hàng xóm của nút`2`, vậy câu trả lời là`NO`. 

Ví dụ này cho thấy rằng có đường dẫn đến đích là chưa đủ. Con bò có thể can thiệp sau nước đi đầu tiên. 

### Mẫu 2 

đầu vào:```
2 1 2
1 2
```Dấu vết là: 

| Nút hiện tại | Hàng xóm đã kiểm tra | Là đích đến? | 
| --- | --- | --- | 
| 1 | 2 | Có | 

Đến đích ngay lần di chuyển đầu tiên, trước khi con bò có thể chặn được bất cứ thứ gì. 

Điều này xác nhận ngoại lệ đầu tiên xác định toàn bộ giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng danh sách kề chạm vào mỗi cạnh một lần, sau đó kiểm tra các lân cận của`s`là tuyến tính ở mức độ`s`. | 
| Không gian | O(n) | Danh sách kề lưu trữ các cạnh của cây. | 

Giải pháp phù hợp với`200000`giới hạn nút vì nó chỉ thực hiện một lượng công việc tuyến tính và lưu trữ cây một lần. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("""5 2 4
1 4
2 5
4 3
3 5
""") == "NO\n", "sample 1"

assert run("""2 1 2
1 2
""") == "YES\n", "sample 2"

# minimum size
assert run("""2 2 1
1 2
""") == "YES\n", "two nodes"

# longer chain
assert run("""4 1 4
1 2
2 3
3 4
""") == "NO\n", "chain longer than one edge"

# star with unrelated branches
assert run("""5 1 5
1 2
1 3
1 4
1 5
""") == "YES\n", "destination is immediate neighbor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút | CÓ | Nước đi đầu tiên thắng ngay lập tức. | 
| Chuỗi bốn nút | KHÔNG | Một con đường dài hơn luôn có thể bị chặn. | 
| Cây sao | CÓ | Việc phân nhánh không thành vấn đề khi mục tiêu ở gần. | 

## Vỏ cạnh 

Để kết nối trực tiếp giữa điểm bắt đầu và điểm đến:```
2 1 2
1 2
```Danh sách lân cận của nút`1`chứa nút`2`. Thuật toán in`YES`bởi vì Spyrosaliv đến được mục tiêu trước khi con bò đến lượt. 

Đối với điểm đến cần di chuyển nhiều lần:```
3 1 3
1 2
2 3
```Nút`1`chỉ kiểm tra hàng xóm`2`. Từ`2`không phải là đích đến, thuật toán sẽ in`NO`. Con bò có thể chặn cạnh từ nút`2`đến nút`3`ngay sau hành động đầu tiên. 

Đối với cây có nhiều cành:```
5 1 5
1 2
1 3
1 4
1 5
```Nút xuất phát có thể có một số bước di chuyển, nhưng một trong số chúng sẽ đến đích ngay lập tức. Thuật toán phát hiện nút`5`giữa những người hàng xóm và bản in`YES`. 

Đối với một cây mà nút bắt đầu không có kết nối trực tiếp tới đích thì số lượng tuyến đường thay thế không thành vấn đề. Mỗi tuyến đường trong cây cuối cùng phải sử dụng cạnh duy nhất tiếp cận`d`, và con bò luôn có thể chặn cạnh đó sau nước đi trước.
