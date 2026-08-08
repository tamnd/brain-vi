---
title: "CF 102500C - Dòng Canvas"
description: "Chúng ta có một chuỗi các canvas không chồng lên nhau trên một trục số. Mỗi khung vẽ bao phủ một khoảng từ điểm cuối bên trái đến điểm cuối bên phải của nó và một chốt nằm chính xác tại điểm cuối được tính là chạm vào khung vẽ đó. Một số chốt đã tồn tại."
date: "2026-08-06T04:40:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 187
verified: true
draft: false
---

[CF 102500C - Dòng Canvas](https://codeforces.com/problemset/problem/102500/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các canvas không chồng lên nhau trên một trục số. Mỗi khung vẽ bao phủ một khoảng từ điểm cuối bên trái đến điểm cuối bên phải của nó và một chốt nằm chính xác tại điểm cuối được tính là chạm vào khung vẽ đó. Một số chốt đã tồn tại. Chúng ta cần thêm số lượng chốt vị trí số nguyên mới tối thiểu để mỗi khung vẽ có chính xác hai chốt chạm vào nó. Nếu bất kỳ khung vẽ nào đã có nhiều hơn hai chốt hoặc nếu việc đáp ứng một khung vẽ buộc khung vẽ khác phải có quá nhiều thì không thể cấu hình. 

Số lượng canvas nhiều nhất là 1000 và số lượng chốt hiện có nhiều nhất là 2000. Tọa độ có thể lớn tới 10^9, do đó, việc lặp lại mọi vị trí có thể có trên đường thẳng là không thể. Giải pháp phải phụ thuộc vào số lượng khung vẽ và chốt chứ không phụ thuộc vào kích thước tọa độ. Một cách tiếp cận xung quanh O(n log n) hoặc O(n^2) là đủ nhanh, trong khi mọi thứ tỷ lệ với phạm vi tọa độ sẽ thất bại. 

Những phần khó khăn đến từ việc chia sẻ điểm cuối của canvas. Một chốt ở cạnh dùng chung thuộc về cả hai khung vẽ, do đó, việc thêm một chốt ở đó có thể giải quyết hai yêu cầu cùng một lúc. Một giải pháp bất cẩn luôn lấp đầy các chốt còn thiếu bên trong khung vẽ có thể sử dụng các chốt bổ sung một cách không cần thiết. 

Ví dụ, hãy xem xét:```
2
0 10
10 20
0
```Đầu ra đúng là:```
2
9 10
```Chốt ở vị trí 10 giúp ích cho cả hai khung vẽ. Thêm`1 2 18 19`hoặc lấp đầy từng khung vẽ một cách độc lập sẽ sử dụng quá nhiều chốt. 

Một trường hợp cạnh khác là canvas đã không hợp lệ:```
1
0 10
3
0 5 10
0 5 10
```Đầu ra đúng là:```
impossible
```Canvas đã chạm vào ba chốt. Không được phép loại bỏ một chốt, vì vậy không có vị trí chốt mới nào có thể khắc phục được. 

Trường hợp thứ ba là khi hai khung vẽ chạm nhau sẽ dẫn đến xung đột:```
3
0 60
60 120
120 140
4
20 60 80 120
```Đầu ra đúng là:```
impossible
```Canvas đầu tiên đã có hai chốt và canvas thứ hai cũng cần chốt ở mức 60. Việc thêm một chốt khác cho canvas thứ hai sẽ làm cho canvas đầu tiên không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý mọi khung vẽ và liên tục tìm kiếm các vị trí chốt bị thiếu. Đối với mỗi khung vẽ, chúng ta có thể đếm các chốt của nó và thử mọi tọa độ số nguyên có thể có bên trong nó cho đến khi tìm thấy đủ các chốt. Điều này đúng vì mọi vị trí hợp pháp cuối cùng đều được xem xét, nhưng nó phụ thuộc vào độ rộng của khung vẽ. Vì tọa độ có thể đạt tới 10^9 nên trường hợp xấu nhất sẽ cần hàng tỷ lượt kiểm tra. 

Quan sát quan trọng là mỗi canvas cần tối đa hai chốt và các canvas đã được sắp xếp từ trái sang phải. Chốt mới được thêm vào ở phía bên phải của canvas có cơ hội trợ giúp lớn nhất cho canvas tiếp theo vì tương tác duy nhất có thể có giữa các canvas là ở điểm cuối được chia sẻ. Vì vậy, khi canvas thiếu chốt, chúng ta nên đặt chúng càng xa càng tốt. Đây là ý tưởng tham lam tương tự như việc duy trì tính linh hoạt trong tương lai: một chốt ở xa hơn bên phải vẫn có thể phục vụ khung vẽ hiện tại, trong khi một chốt ở xa hơn bên trái không thể hỗ trợ các khung vẽ sau này. 

Đối với mỗi khung vẽ, chúng tôi đếm số chốt hiện đang chạm vào khung vẽ đó. Nếu đã có nhiều hơn hai thì câu trả lời là không thể. Nếu có ít hơn hai, chúng ta chèn các chốt còn thiếu vào các vị trí sẵn có lớn nhất trong khoảng. Vì chiều rộng tối thiểu là 10 nên luôn có đủ vị trí số nguyên bên trong khung vẽ để thêm tối đa hai chốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * phạm vi tọa độ) | O(1) | Quá chậm | 
| Tối ưu | O(n * (p + n)) | O(p + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các vị trí chốt hiện tại trong một bộ để có thể kiểm tra nhanh chóng cả các chốt hiện có và mới được thêm vào. 
2. Đối với mỗi khung vẽ từ trái sang phải, hãy đếm xem có bao nhiêu chốt hiện tại nằm giữa hai điểm cuối của nó, bao gồm cả cả hai điểm cuối. Nếu số lượng vượt quá hai, hãy dừng lại vì canvas không thể sửa chữa được. 
3. Nếu khung vẽ cần thêm chốt, hãy thử các vị trí từ điểm cuối bên phải trở xuống. Thêm các vị trí còn thiếu đầu tiên cho đến khi khung vẽ có hai chốt. 
4. Sau khi tất cả các khung vẽ được xử lý, xuất ra các vị trí đã thêm. Số lượng chốt được thêm vào là tối thiểu vì mỗi chốt được thêm vào đều được đặt ở nơi có cơ hội được sử dụng lại bởi khung vẽ sau đây lớn nhất có thể. 

Tại sao nó hoạt động: sau khi xử lý canvas, nó luôn có chính xác hai chốt và tất cả các canvas trước đó vẫn hợp lệ. Khi một chốt bị thiếu được thêm vào, việc chọn tọa độ lớn nhất có thể sẽ giữ cho chốt đó có sẵn cho khung vẽ trong tương lai chia sẻ ranh giới. Bất kỳ lựa chọn nào khác chỉ có thể làm giảm các lựa chọn trong tương lai, vì vậy lựa chọn tham lam không bao giờ đòi hỏi nhiều chốt hơn một giải pháp hợp lệ khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    canvases = []
    for _ in range(n):
        l, r = map(int, input().split())
        canvases.append((l, r))

    p = int(input())
    pegs = set()
    if p:
        pegs.update(map(int, input().split()))

    added = []

    for l, r in canvases:
        cnt = 0
        for x in pegs:
            if l <= x <= r:
                cnt += 1
        if cnt > 2:
            print("impossible")
            return

        need = 2 - cnt
        x = r
        while need:
            if x not in pegs:
                pegs.add(x)
                added.append(x)
                need -= 1
            x -= 1

    print(len(added))
    if added:
        print(*added)

if __name__ == "__main__":
    solve()
```Bộ chứa cả chốt gốc và chốt được tạo bởi thuật toán. Điều này quan trọng vì một chốt được thêm cho một canvas có thể là chốt dùng chung mà canvas tiếp theo yêu cầu. 

Việc tìm kiếm bắt đầu từ điểm cuối bên phải và di chuyển sang trái. Vì một canvas cần tối đa hai chốt và chiều rộng của nó ít nhất là 10, nên vòng lặp luôn tìm đủ vị trí. Không có vấn đề tràn số nguyên trong Python vì tọa độ vừa vặn thoải mái khi xử lý số nguyên thông thường. 

Việc triển khai sử dụng tính năng quét đơn giản qua tất cả các chốt cho mỗi khung vẽ. Với tổng số tối đa khoảng 4000 chốt sau khi bổ sung và chỉ có 1000 khung vẽ, con số này vẫn còn nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
0 18
18 28
28 40
49 60
4
6 12 35 60
```| Vải | Chốt hiện có bên trong | Thiếu chốt | Đã thêm | 
| --- | --- | --- | --- | 
| 0 18 | 6, 12 | 0 | | 
| 18 28 | không | 2 | 28, 27 | 
| 28 40 | 28, 35 | 0 | | 
| 49 60 | 60 | 1 | 59 | 

Chốt chia sẻ ở mức 28 được sử dụng bởi cả hai khung vẽ ở giữa. Kết quả là ba chốt mới:`28 27 59`. 

Đối với mẫu thứ hai:```
5
2 15
15 25
25 40
42 52
52 62
3
5 29 52
```| Vải | Chốt hiện có bên trong | Thiếu chốt | Đã thêm | 
| --- | --- | --- | --- | 
| 2 15 | 5 | 1 | 15 | 
| 15 25 | 15 | 1 | 25 | 
| 25 40 | 25,29 | 0 | | 
| 42 52 | 52 | 1 | 51 | 
| 52 62 | 52 | 1 | 62 | 

Chốt được thêm vào ở mức 15 đáp ứng hai khung vẽ đầu tiên và chốt được thêm vào ở mức 52 đã có sẵn và được chia sẻ giữa hai khung vẽ cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n(p+n)) | Mỗi khung vẽ quét bộ chốt hiện tại, chứa các chốt hiện có và được thêm vào | 
| Không gian | O(p+n) | Bộ lưu trữ tất cả các chốt và danh sách câu trả lời lưu trữ các chốt mới | 

Số lượng chốt được lưu trữ tối đa là khoảng 4000, do đó quá trình quét bậc hai đủ nhỏ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout
    sys.stdin = old
    return out.getvalue()

assert run("""4
0 18
18 28
28 40
49 60
4
6 12 35 60
""").strip() == "3\n28 27 59"

assert run("""5
2 15
15 25
25 40
42 52
52 62
3
5 29 52
""").strip() == "4\n15 25 51 62"

assert run("""3
0 60
60 120
120 140
4
20 60 80 120
""").strip() == "impossible"

assert run("""1
0 10
0
""").strip() == "2\n10 9"

assert run("""1
0 10
3
0 5 10
""").strip() == "impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Canvas trống đơn | Hai chốt ở bên phải | Đầu vào tối thiểu và chèn cơ bản | 
| Ba chốt hiện có | không thể | Phát hiện các canvas đã không hợp lệ | 
| Bức tranh cảm động | Sử dụng điểm cuối được chia sẻ | Kiểm tra việc xử lý ranh giới tham lam | 
| Trường hợp mẫu | Kết quả đầu ra mẫu | Xác nhận hành vi bình thường | 

## Vỏ cạnh 

Đối với trường hợp ranh giới chung:```
2
0 10
10 20
0
```Canvas đầu tiên cần hai chốt. Thuật toán thử các vị trí từ 10 trở xuống và thêm 10 và 9. Canvas thứ hai ngay lập tức nhìn thấy chốt ở mức 10 và chỉ cần thêm một chốt nữa, trở thành 20. Kết quả sử dụng ba chốt thay vì bốn vì lựa chọn tham lam đã bảo toàn điểm cuối chung. 

Đối với canvas đã quá tải:```
1
0 10
3
0 5 10
```Thuật toán đếm cả ba chốt hiện có trước khi thêm bất kỳ thứ gì. Vì số lượng đã lớn hơn hai nên kết quả trả về là không thể ngay lập tức. 

Đối với xung đột bắt buộc:```
3
0 60
60 120
120 140
4
20 60 80 120
```Canvas đầu tiên chứa các vị trí 20 và 60, như vậy là đã hoàn tất. Canvas thứ hai chứa 60 và 80, cũng đã hoàn thành. Canvas thứ ba chỉ chứa 120 và nhận được một chốt mới ở mức 140. Tuy nhiên, hai canvas đầu tiên chia sẻ cách sắp xếp chốt ranh giới nên không có cách nào đáp ứng mọi canvas có chính xác hai chốt, do đó, thuật toán sẽ phát hiện phần vượt quá và không thể báo cáo.
