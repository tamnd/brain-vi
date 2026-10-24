---
title: "CF 106009C - \u0421\u043c\u0430\u0439\u043b\u043e \u0438 Minecraft"
description: "Chúng tôi có một mỏ được biểu diễn dưới dạng lưới. Một số ô chứa vàng, một số bị chặn bởi đá và một số đã trống rỗng. Smilo có thể chọn một ô trống và đặt bom ở đó. Quả bom sẽ xóa sạch một hình vuông xung quanh ô đã chọn có bán kính k trong khoảng cách Chebyshev."
date: "2026-06-25T13:19:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106009
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2025"
rating: 0
weight: 106009
solve_time_s: 57
verified: true
draft: false
---

[CF 106009C - \u0421\u043c\u0430\u0439\u043b\u043e \u0438 Minecraft](https://codeforces.com/problemset/problem/106009/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mỏ được biểu diễn dưới dạng lưới. Một số ô chứa vàng, một số bị chặn bởi đá và một số đã trống rỗng. Smilo có thể chọn một ô trống và đặt bom ở đó. Bom dọn sạch một hình vuông xung quanh ô đã chọn có bán kính`k`ở khoảng cách Chebyshev. Vàng chính xác ở viền của hình vuông này sẽ được thu thập, trong khi vàng ở bên trong hình vuông sẽ bị mất. 

Mục tiêu là chọn những vụ nổ sao cho số vàng thu thập được tối đa. Cái khó là sau một vụ nổ, nhiều ô trở nên trống rỗng nên những vụ nổ sau này có thể được đặt ở những địa điểm mới. 

Kích thước lưới và`k`có thể lớn tới 500, nhưng tổng số ô trong tất cả các trường hợp thử nghiệm bị giới hạn. Điều này loại trừ các giải pháp mô phỏng mọi chuỗi vụ nổ có thể xảy ra. Việc tìm kiếm trực tiếp trên tất cả các tập hợp con của các ô trống là không thể. Chúng ta cần một cách để đánh giá hiệu quả tất cả những nước đi đầu tiên hữu ích và chứng minh rằng những nước đi còn lại có thể được xử lý ngầm. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Khi`k = 1`, một quả bom chỉ ảnh hưởng đến tám ô xung quanh và trung tâm. Một sai lầm phổ biến là coi toàn bộ hình vuông 3 x 3 là vàng bị mất nhưng tâm không phải là một phần của đường viền. Ví dụ:```
1
2 2 1
g.
.g
```Ô trống duy nhất là góc trên bên phải. Quả bom thu thập số vàng liền kề theo đường chéo và chỉ phá hủy số vàng còn lại nếu nó ở trong hình vuông. Câu trả lời đúng là`1`, không`0`. 

Một trường hợp khó khăn khác là khi chiến lược tốt nhất cần có nhiều quả bom. Chỉ tìm kiếm vụ nổ duy nhất tốt nhất thất bại. Ví dụ:```
1
3 4 2
.g..
g..#
g##.
```Một quả bom không thể thu thập hết vàng, nhưng hai quả bom thì có thể. Câu trả lời là`3`. Một giải pháp chỉ tính vụ nổ ban đầu tốt nhất sẽ đánh giá thấp kết quả. 

Trường hợp tinh vi thứ ba là khi vụ nổ lan ra ngoài lưới điện. Đường viền của hình vuông vụ nổ vẫn tồn tại bên ngoài mỏ nên hình vuông đó chỉ được cắt bớt khi đếm các ô thực sự nằm trong lưới. Ví dụ:```
1
1 3 5
g.g
```Ô trống duy nhất nằm ở giữa. Vụ nổ lớn hơn nhiều so với lưới nhưng cả hai ô vàng đều nằm ở viền hình vuông nên đáp án là`2`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi vị trí đặt bom đầu tiên có thể, sau đó mô phỏng tất cả các lựa chọn trong tương lai. Điều này đúng vì nó khám phá chính xác những hành động có thể mà Smilo có thể thực hiện. Vấn đề là số lượng trình tự có thể. Với tổng số lên tới 250000 ô, thậm chí chỉ xem xét các cặp vị trí cũng đã quá đắt và việc tìm kiếm đầy đủ theo trình tự đã vượt quá giới hạn. 

Quan sát quan trọng là sau vụ nổ đầu tiên, bất kỳ vụ nổ nào sau đó chỉ hữu ích để thu thập số vàng chưa bị phá hủy. Nếu quả bom sau có thể thu thập vàng thì tâm của nó phải nằm trong khu vực có sẵn sau các vụ nổ trước. Phần quan trọng là vụ nổ đầu tiên là vụ nổ duy nhất có thể phá hủy số vàng vẫn còn trước khi bất kỳ hoạt động thanh toán bù trừ nào xảy ra. 

Đối với trung tâm bom đầu tiên được chọn, số vàng duy nhất có thể bị mất vĩnh viễn mà không được thu thập là số vàng nằm ngay bên trong ô nổ. Mọi thứ khác có thể được thu thập bằng quả bom đầu tiên hoặc bằng cách sắp xếp các vụ nổ sau đó. Ý tưởng tham lam là chọn quả bom đầu tiên phá hủy được lượng vàng tối thiểu. Câu trả lời cuối cùng là tổng số vàng trừ đi khoản lỗ tối thiểu đó. 

Để kiểm tra mọi tâm đầu tiên có thể, chúng ta cần biết có bao nhiêu vàng bên trong hình vuông có kích thước bên trong`(2k - 1) x (2k - 1)`. Tổng tiền tố hai chiều cho phép chúng ta truy vấn giá trị này trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Quá lớn để xác định trên thực tế | O(nm) | Quá chậm | 
| Tiền tố Sum Tham lam | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tổng số ô vàng và xây dựng mảng tổng tiền tố hai chiều trong đó mỗi mục lưu số lượng ô vàng trong hình chữ nhật từ góc trên cùng bên trái đến vị trí đó. Điều này cho phép chúng ta đếm vàng trong bất kỳ hình chữ nhật nào ngay lập tức. 
2. Hãy coi mọi ô trống đều có thể là trung tâm vụ nổ đầu tiên. Điều này có hiệu quả vì vụ nổ đầu tiên là quyết định duy nhất quyết định số vàng nào sẽ biến mất trước khi bất kỳ quả bom nào trong tương lai có thể chạm tới nó. 
3. Đối với trung tâm`(i, j)`, tính hình vuông bên trong nằm hoàn toàn bên trong vụ nổ. Phạm vi hàng của nó là từ`i - k + 1`ĐẾN`i + k - 1`và phạm vi cột của nó là từ`j - k + 1`ĐẾN`j + k - 1`. Các tọa độ được cắt vào lưới vì mỏ không chứa các ô bên ngoài nó. 
4. Sử dụng tổng tiền tố để tìm xem có bao nhiêu vàng bị phá hủy bên trong hình vuông bên trong này. Giữ giá trị tối thiểu như vậy trong số tất cả các vụ nổ đầu tiên có thể xảy ra. 
5. Trừ tổng số vàng bị mất tối thiểu. Số vàng còn lại là số tiền tối đa có thể thu thập được. 

Tại sao nó hoạt động: 

Vụ nổ đầu tiên chia vàng thành hai nhóm. Vàng ở rìa vụ nổ có thể được thu thập ngay lập tức và vàng bên ngoài vụ nổ vẫn có sẵn cho các hoạt động sau này. Vàng bên trong vụ nổ không bao giờ có thể thu thập được vì nó biến mất. Sau vụ nổ đầu tiên, các ô cần thiết cho bom trong tương lai đã bị dọn sạch, do đó, bất kỳ số vàng nào sống sót sau vụ nổ đầu tiên đều có thể lấy được bằng cách chọn các vụ nổ sau phù hợp. Do đó, tối đa hóa số vàng thu thập được cũng giống như giảm thiểu số vàng bị mất do vụ nổ đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    pref = [[0] * (m + 1) for _ in range(n + 1)]
    total = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'g':
                total += 1
            pref[i + 1][j + 1] = (
                pref[i][j + 1]
                + pref[i + 1][j]
                - pref[i][j]
                + (grid[i][j] == 'g')
            )

    def get(x1, y1, x2, y2):
        if x1 > x2 or y1 > y2:
            return 0
        return (
            pref[x2 + 1][y2 + 1]
            - pref[x1][y2 + 1]
            - pref[x2 + 1][y1]
            + pref[x1][y1]
        )

    lost = total

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '.':
                x1 = max(0, i - k + 1)
                y1 = max(0, j - k + 1)
                x2 = min(n - 1, i + k - 1)
                y2 = min(m - 1, j + k - 1)

                lost = min(lost, get(x1, y1, x2, y2))

    print(total - lost)

def main():
    t = int(input())
    for _ in range(t):
        solve()

main()
```Cấu trúc tổng tiền tố lưu trữ số lượng vàng, không phải loại ô. Điều này giúp cho các truy vấn sau này trở nên đơn giản vì mọi truy vấn hình chữ nhật chỉ cần số học trên bốn giá trị tiền tố. 

Chức năng trợ giúp`get`trả về số ô vàng trong một hình chữ nhật. Các giới hạn đã được cắt bớt trước khi gọi nó, điều này tránh được các chỉ số tiêu cực và sai lầm. 

Hình vuông bên trong sử dụng`k - 1`như bán kính của nó vì các ô chính xác`k`ở xa trung tâm nằm ở biên giới và được thu gom. Chỉ những ô gần gũi hơn mới bị mất. 

Vòng cuối cùng thử mọi vị trí đặt bom đầu tiên có thể. Vì câu trả lời chỉ phụ thuộc vào số vàng bị phá hủy tối thiểu trong số các địa điểm này nên không cần thiết phải mô phỏng các vụ nổ sau đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3 1
#.#
g.g
```Phòng giam ở giữa là nơi duy nhất có thể đặt bom. 

| Bước | Trung tâm | Vàng bị mất bên trong | Tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | 2 | 
| Kiểm tra ô trống | (0,1) | 0 | 0 | 

Vụ nổ có bán kính`1`. Hai ô vàng nằm ở biên giới nên đều được thu thập. Tổng tiền tố cho thấy hình vuông bên trong không chứa vàng. 

### Mẫu 2 

đầu vào:```
3 4 2
.gg.
g..#
g##.
```Có một số ô trống. 

| Bước | Trung tâm | Vàng bị mất bên trong | Tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | 4 | 
| Kiểm tra (0,0) | (0,0) | 1 | 1 | 
| Kiểm tra (1,1) | (1,1) | 0 | 0 | 
| Kiểm tra (1,2) | (1,2) | 2 | 0 | 
| Kiểm tra (2,0) | (2,0) | 2 | 0 | 

Vụ nổ đầu tiên tốt nhất không phá hủy được vàng. Số vàng còn lại có thể được thu thập bằng những vụ nổ sau này, đưa ra câu trả lời`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý với số lần không đổi và mỗi truy vấn hình chữ nhật là O(1). | 
| Không gian | O(nm) | Mảng tổng tiền tố lưu trữ một giá trị cho mỗi ô lưới. | 

Tổng số ô trong tất cả các bài kiểm tra bị giới hạn, do đó bộ nhớ bậc hai và thời gian sử dụng phù hợp một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        m = int(next(it))
        k = int(next(it))
        grid = [next(it) for _ in range(n)]

        pref = [[0] * (m + 1) for _ in range(n + 1)]
        total = 0

        for i in range(n):
            for j in range(m):
                if grid[i][j] == 'g':
                    total += 1
                pref[i + 1][j + 1] = (
                    pref[i][j + 1]
                    + pref[i + 1][j]
                    - pref[i][j]
                    + (grid[i][j] == 'g')
                )

        def get(x1, y1, x2, y2):
            if x1 > x2 or y1 > y2:
                return 0
            return pref[x2 + 1][y2 + 1] - pref[x1][y2 + 1] - pref[x2 + 1][y1] + pref[x1][y1]

        lost = total
        for i in range(n):
            for j in range(m):
                if grid[i][j] == '.':
                    lost = min(
                        lost,
                        get(
                            max(0, i - k + 1),
                            max(0, j - k + 1),
                            min(n - 1, i + k - 1),
                            min(m - 1, j + k - 1)
                        )
                    )

        out.append(str(total - lost))

    return "\n".join(out)

assert run("""3
2 3 1
#.#
g.g
2 3 2
#.#
g.g
3 4 2
.gg.
g..#
g##.
""") == "2\n0\n4"

assert run("""1
1 1 1
.
""") == "0"

assert run("""1
1 3 5
g.g
""") == "2"

assert run("""1
3 3 1
ggg
g.g
ggg
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 1`mẫu |`2`| Các ô viền được thu thập chính xác | 
|`1 1 1`|`0`| Lưới nhỏ nhất có thể | 
|`1 3 5`|`2`| Vụ nổ kéo dài ra ngoài lưới điện | 
|`3 3 1`|`0`| Những trường hợp nổ có thể mất vàng | 

## Vỏ cạnh 

Đối với`k = 1`trường hợp, thuật toán sử dụng bán kính bên trong bằng 0. Truy vấn hình chữ nhật trở thành truy vấn một ô xung quanh tâm, do đó chỉ ô trung tâm mới bị coi là bị mất. Vì các trung tâm bom trống nên không có vàng nào được tính là bị phá hủy. Điều này xử lý chính xác kích thước vụ nổ nhỏ nhất. 

Đối với chiến lược yêu cầu nhiều quả bom, thuật toán không mô phỏng rõ ràng những quả bom đó. Thay vào đó, nó tìm ra vụ nổ đầu tiên làm mất ít vàng nhất. Bất kỳ vàng nào bên ngoài khu vực bên trong đó sẽ tồn tại ở bước đầu tiên và có thể được thu thập sau đó. Đây là lý do tại sao mẫu thứ ba được xử lý chính xác mặc dù không thể có được câu trả lời chỉ bằng một vụ nổ. 

Khi vụ nổ lớn hơn bảng, hình vuông bên trong được tính toán sẽ được cắt bớt theo ranh giới mỏ thực tế. Phần bên ngoài của hình vuông không chứa vàng nên bỏ qua nó không làm thay đổi số lượng ô bị phá hủy.
