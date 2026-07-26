---
title: "CF 102862L - Hộp Rơi"
description: "Chúng ta có một bộ sưu tập các hộp vuông giống hệt nhau được đặt bên trong một khối lưu trữ được làm từ hai bức tường chéo. Hệ tọa độ mô tả mỗi hộp theo hai chỉ số: một lớp đếm từ bức tường bên trái và một lớp đếm từ bức tường bên phải."
date: "2026-07-25T13:57:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "L"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 50
verified: true
draft: false
---

[CF 102862L - Hộp Rơi](https://codeforces.com/problemset/problem/102862/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một bộ sưu tập các hộp vuông giống hệt nhau được đặt bên trong một khối lưu trữ được làm từ hai bức tường chéo. Hệ tọa độ mô tả mỗi hộp theo hai chỉ số: một lớp đếm từ bức tường bên trái và một lớp đếm từ bức tường bên phải. Bởi vì sự sắp xếp ổn định, các vị trí chiếm giữ tạo thành một hình dạng giống như cầu thang, trong đó mọi hộp hỗ trợ hộp khác đều đã có sẵn. 

Hộp thấp nhất, hộp chạm vào cả hai bức tường, sẽ bị loại bỏ. Vị trí trống nó để lại sẽ trở thành một cái lỗ. Trọng lực làm cho lỗ di chuyển lên trên qua đống: hoặc hộp ngay phía trên nó ở một bên rơi xuống, hoặc hộp ngay phía trên nó ở phía bên kia rơi xuống. Sau khi hộp rơi xuống, lỗ sẽ di chuyển về vị trí trước đó của hộp. Quá trình dừng lại khi lỗ đạt đến vị trí không có hộp nào có thể rơi vào đó. 

Nhiệm vụ là đếm xem có thể có bao nhiêu chuỗi lựa chọn rơi khác nhau. Câu trả lời phải được in modulo$10^9+7$. 

Đầu vào chứa tối đa$10^5$các hộp và mọi tọa độ tối đa là$n$. Một mô phỏng bậc hai là không thể bởi vì$10^{10}$các hoạt động sẽ được yêu cầu trong trường hợp lớn nhất. Chúng ta cần một giải pháp chỉ xử lý mỗi hộp một số lần nhỏ, hướng tới một$O(n)$hoặc$O(n \log n)$tiếp cận. 

Các trường hợp đặc biệt chính xuất phát từ việc nhầm lẫn các vị trí cuối cùng với các chuỗi rơi xuống. Những lựa chọn khác nhau có thể dẫn đến những lịch sử khác nhau ngay cả khi hố kết thúc ở cùng một vị trí. Ví dụ, hãy xem xét:```
5
1 1
1 2
2 1
2 2
3 1
```Câu trả lời đúng là:```
3
```Một cách tiếp cận bất cẩn có thể chỉ đếm các ô trống cuối cùng có thể. Lỗ có thể kết thúc tại`(2,2)`hoặc`(3,1)`, cho hai trạng thái cuối cùng, nhưng có thể có ba chuỗi rơi xuống vì lỗ có thể chạm tới`(2,2)`theo hai cách khác nhau. 

Một trường hợp góc khác là một hộp duy nhất:```
1
1 1
```Câu trả lời là:```
1
```Không có lựa chọn nào để thực hiện. Tình huống duy nhất có thể xảy ra là chiếc hộp bị loại bỏ sẽ để lại một phòng chứa đồ trống. 

Trường hợp thứ ba là cọc thẳng:```
3
1 1
2 1
3 1
```Câu trả lời là:```
1
```Mỗi vị trí lỗ có chính xác một hộp có thể rơi xuống, vì vậy quá trình này không có sự phân nhánh. 

# Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ tuân theo mọi chuỗi rơi có thể xảy ra. Bắt đầu từ hộp dưới cùng đã bị loại bỏ, chúng ta thử đệ quy mọi hộp có thể lấp đầy lỗ trống. Điều này đúng vì mọi kịch bản hợp lệ đều chính xác là một chuỗi các lựa chọn này. 

Vấn đề là đệ quy này có thể phân nhánh nhiều lần. Trong một đống hình cầu thang dày đặc, có thể tồn tại nhiều con đường khác nhau và việc khám phá từng con đường riêng lẻ có thể mất thời gian theo cấp số nhân. Các vị trí lỗ giống nhau được đạt tới nhiều lần theo các trình tự khác nhau, do đó phương pháp vũ lực lặp lại hoạt động. 

Nhận xét quan trọng là điều duy nhất quan trọng ở bất kỳ thời điểm nào là vị trí hiện tại của lỗ. Nếu lỗ đạt đến một vị trí ô nhất định, số cách để đến đó sẽ không phụ thuộc vào trình tự chính xác đã tạo ra nó. Điều này cho phép chúng tôi hợp nhất tất cả các trạng thái tương đương. 

Sự cố trở thành vấn đề lập trình động trên các ô bị chiếm dụng. Một ô nhận được tổng số cách từ các ô bên dưới nó trong quá trình rơi xuống. Vì lỗ chỉ di chuyển bằng cách tăng một tọa độ nên đồ thị không có chu trình. Chúng ta có thể xử lý các tế bào theo thứ tự tăng dần`x + y`. 

Đối với mỗi ô, chúng tôi lưu trữ số cách mà lỗ có thể đến đó. Nếu một ô không thể có bước đi tiếp theo, nó thể hiện một kịch bản đã hoàn thành, vì vậy giá trị của nó góp phần đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng tế bào phân nhánh | Độ sâu đệ quy O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi vị trí hộp đã sử dụng trong một tập hợp sao cho việc kiểm tra xem hộp lân cận có tồn tại hay không mất thời gian không đổi. Các tọa độ tạo thành một biểu đồ tuần hoàn có hướng vì mỗi bước di chuyển đều tăng`x`hoặc`y`. 
2. Sắp xếp các ô theo thứ tự tăng dần`x + y`. Điều này mang lại một thứ tự hợp lệ cho lập trình động vì một lỗ chỉ có thể di chuyển từ tổng tọa độ nhỏ hơn sang tổng tọa độ lớn hơn. 
3. Đặt số cách để đạt đến vị trí đáy đã loại bỏ`(1,1)`đến một. Điều này thể hiện sự bắt đầu của quá trình rơi. 
4. Truy cập từng ô theo thứ tự sắp xếp. Đối với vị trí hiện tại`(x,y)`, cộng số cách hiện tại của nó để`(x+1,y)`nếu hộp đó tồn tại. Điều này tương ứng với việc hộp từ phía bên trái rơi vào lỗ. 
5. Thêm số cách hiện tại để`(x,y+1)`nếu hộp đó tồn tại. Điều này tương ứng với việc hộp từ phía bên phải rơi vào lỗ. 
6. Sau khi tất cả các chuyển đổi được xử lý, tính tổng các giá trị của mỗi hộp không có hộp nào ngay bên dưới nó theo một trong hai hướng. Đây là những vị trí mà lỗ dừng lại, vì vậy mọi đường đi kết thúc đều có một kịch bản hợp lệ. 

Tại sao nó hoạt động: 

Tính bất biến đó là`dp[x][y]`bằng số chuỗi rơi có thể di chuyển lỗ từ vị trí trống ban đầu sang`(x,y)`. Trạng thái bắt đầu có một chuỗi trống. Bất cứ khi nào lỗ đến một ô, trạng thái tiếp theo duy nhất có thể xảy ra là hai hộp lân cận có thể rơi vào đó, do đó, việc cộng số lượng hiện tại vào các ô đó sẽ cho mỗi lần tiếp tục có thể xảy ra đúng một lần. Vì mọi kịch bản có thể xảy ra đều là một đường dẫn từ`(1,1)`đến ô đầu cuối, tổng hợp tất cả các trạng thái đầu cuối sẽ đưa ra câu trả lời cuối cùng. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    boxes = []
    box_set = set()

    for _ in range(n):
        x, y = map(int, input().split())
        boxes.append((x, y))
        box_set.add((x, y))

    boxes.sort(key=lambda p: p[0] + p[1])

    dp = {}

    for x, y in boxes:
        dp[(x, y)] = 0

    dp[(1, 1)] = 1

    for x, y in boxes:
        cur = dp[(x, y)]

        if (x + 1, y) in box_set:
            dp[(x + 1, y)] = (dp[(x + 1, y)] + cur) % MOD

        if (x, y + 1) in box_set:
            dp[(x, y + 1)] = (dp[(x, y + 1)] + cur) % MOD

    ans = 0

    for x, y in boxes:
        if (x + 1, y) not in box_set and (x, y + 1) not in box_set:
            ans = (ans + dp[(x, y)]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```bộ`box_set`cung cấp kiểm tra hàng xóm theo thời gian liên tục. Vì chỉ có hai nước đi có thể xảy ra từ mỗi vị trí nên không cần danh sách kề. 

Bước sắp xếp là thứ tự tôpô của đồ thị ẩn. Tọa độ luôn tăng lên trong quá trình rơi, do đó việc xử lý sẽ nhỏ hơn`x + y`các giá trị đầu tiên đảm bảo rằng tất cả các chuyển đổi đến một trạng thái đã được xem xét trước khi nó được sử dụng. 

Từ điển`dp`lưu trữ số lượng đường dẫn đến từng vị trí bị chiếm đóng. Số nguyên Python không bị tràn, nhưng mỗi lần cập nhật đều được giảm modulo$10^9+7$để giữ cho các giá trị được lưu trữ ở mức nhỏ. 

Kiểm tra trạng thái đầu cuối sử dụng chính xác hai bước di chuyển có thể có từ một vị trí. Hộp là điểm dừng nếu không có hộp lân cận nào tồn tại. Việc kiểm tra điều này sau DP sẽ tránh được mọi xử lý đặc biệt trong quá trình chuyển tiếp. 

# Ví dụ đã hoạt động 

Đối với mẫu:```
5
1 1
1 2
2 2
3 1
2 1
```Thứ tự sắp xếp đã có: 

| Tế bào | Cách hiện tại | Thêm vào`(x+1,y)`| Thêm vào`(x,y+1)`| 
| --- | --- | --- | --- | 
|`(1,1)`| 1 |`(2,1)`được 1 |`(1,2)`được 1 | 
|`(1,2)`| 1 |`(2,2)`được 1 | không | 
|`(2,1)`| 1 |`(3,1)`được 1 |`(2,2)`được thêm 1 | 
|`(2,2)`| 2 | không | không | 
|`(3,1)`| 1 | không | không | 

Các tế bào đầu cuối là`(2,2)`Và`(3,1)`. Giá trị của chúng là`2`Và`1`, đưa ra câu trả lời`3`. 

Ví dụ này cho thấy tại sao chỉ tính tổng các vị trí đầu cuối là không đủ. Có thể đạt được cùng một vị trí trống cuối cùng bằng nhiều lựa chọn. 

Một chuỗi thẳng:```
3
1 1
2 1
3 1
```sản xuất: 

| Tế bào | Cách hiện tại | Ô tiếp theo | 
| --- | --- | --- | 
|`(1,1)`| 1 |`(2,1)`| 
|`(2,1)`| 1 |`(3,1)`| 
|`(3,1)`| 1 | dừng lại | 

Chỉ có một con đường duy nhất nên câu trả lời là`1`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hộp được sắp xếp một lần và được xử lý bằng hai lần kiểm tra hàng xóm | 
| Không gian | O(n) | Tập hợp các hộp và bảng DP, mỗi hộp chứa tối đa một mục nhập cho mỗi hộp | 

Sự hạn chế của$10^5$các hộp có nghĩa là giải pháp tuyến tính dễ dàng phù hợp với giới hạn cuộc thi điển hình. Thuật toán không bao giờ khám phá các kịch bản rơi riêng lẻ mà chỉ khám phá các trạng thái được chia sẻ bởi tất cả các kịch bản. 

# Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        data = sys.stdin.readline
        n = int(data())
        boxes = []
        box_set = set()

        for _ in range(n):
            x, y = map(int, data().split())
            boxes.append((x, y))
            box_set.add((x, y))

        boxes.sort(key=lambda p: p[0] + p[1])

        dp = {p: 0 for p in boxes}
        dp[(1, 1)] = 1

        for x, y in boxes:
            cur = dp[(x, y)]
            if (x + 1, y) in box_set:
                dp[(x + 1, y)] = (dp[(x + 1, y)] + cur) % MOD
            if (x, y + 1) in box_set:
                dp[(x, y + 1)] = (dp[(x, y + 1)] + cur) % MOD

        ans = 0
        for x, y in boxes:
            if (x + 1, y) not in box_set and (x, y + 1) not in box_set:
                ans = (ans + dp[(x, y)]) % MOD

        return str(ans) + "\n"
    finally:
        sys.stdin = old_stdin

assert solution("""5
1 1
1 2
2 2
3 1
2 1
""") == "3\n", "sample"

assert solution("""1
1 1
""") == "1\n", "single box"

assert solution("""3
1 1
2 1
3 1
""") == "1\n", "straight pile"

assert solution("""4
1 1
1 2
1 3
2 1
""") == "1\n", "single branch"

assert solution("""6
1 1
1 2
2 1
2 2
3 1
1 3
""") == "4\n", "multiple branches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp đơn |`1`| Không có lựa chọn chuyển động nào tồn tại | 
| Cọc thẳng |`1`| Một chuỗi không phân nhánh | 
| Chi nhánh đơn |`1`| Chỉ có một khả năng tiếp tục | 
| Nhiều chi nhánh |`4`| Một số đường dẫn hợp nhất vào cùng một trạng thái | 

# Vỏ cạnh 

Đối với trường hợp hộp đơn:```
1
1 1
```DP bắt đầu bằng một chiều tại`(1,1)`. Ô không có hàng xóm nên được tính là trạng thái cuối. Câu trả lời vẫn còn`1`. 

Ví dụ có nhiều đường dẫn đến cùng một điểm kết thúc:```
5
1 1
1 2
2 1
2 2
3 1
```Thuật toán lưu trữ hai cách khác nhau để tiếp cận`(2,2)`riêng biệt thông qua bổ sung DP. Khi ô được xử lý, giá trị của nó là`2`, không`1`, vì vậy cả hai lịch sử đều được tính. 

Đối với cọc thẳng:```
3
1 1
2 1
3 1
```Mỗi trạng thái có chính xác một cạnh đi ra. Giá trị DP vẫn còn`1`qua toàn bộ chuỗi và chỉ ô cuối cùng mới góp phần đưa ra câu trả lời. Điều này ngăn chặn việc đếm quá mức khi không có phân nhánh.
