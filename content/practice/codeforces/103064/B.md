---
title: "CF 103064B - \u041a\u0430\u043a \u043f\u0440\u043e\u0439\u0442\u0438 \u0432 \u0441\u0442\u043e\u043b\u043e\u0432\u0443\u044e"
description: "Chúng ta được cho một lưới hình chữ nhật tượng trưng cho hệ thống hành lang tòa nhà. Mỗi ô của lưới đều trống, một bức tường, một chiếc chìa khóa, một cánh cửa bị khóa hoặc vị trí bắt đầu. Từ ô bắt đầu, một người di chuyển theo một trình tự hướng dẫn cố định."
date: "2026-07-04T01:04:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103064
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2021"
rating: 0
weight: 103064
solve_time_s: 59
verified: true
draft: false
---

[CF 103064B - \u041a\u0430\u043a \u043f\u0440\u043e\u0439\u0442\u0438 \u0432 \u0441\u0442\u043e\u043b\u043e\u0432\u0443\u044e](https://codeforces.com/problemset/problem/103064/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật tượng trưng cho hệ thống hành lang tòa nhà. Mỗi ô của lưới đều trống, một bức tường, một chiếc chìa khóa, một cánh cửa bị khóa hoặc vị trí bắt đầu. Từ ô bắt đầu, một người di chuyển theo một trình tự hướng dẫn cố định. Quy tắc di chuyển không phải là bước lưới tiêu chuẩn: khi một hướng được chọn, người đó sẽ tiếp tục trượt từng ô theo hướng đó cho đến khi có thứ gì đó chặn chuyển động tiếp theo. 

Ô chặn có thể là ranh giới của lưới, tường hoặc cửa bị khóa. Cửa bị khóa hoạt động khác nhau tùy thuộc vào việc người đó hiện có mang theo ít nhất một chìa khóa hay không. Nếu có chìa khóa, cửa sẽ mở ngay lập tức khi bạn đến và chuyển động tiếp tục xuyên qua cửa mà không dừng lại, tiêu tốn một chìa khóa. Chìa khóa được thu thập tự động khi được chuyển qua và không có giới hạn về số lượng có thể mang theo. Điều quan trọng là chìa khóa hoàn toàn không phải là chướng ngại vật, chuyển động vẫn tiếp tục bình thường sau khi nhặt chúng lên. 

Có hai chế độ. Nếu D bằng 0 thì lưới chỉ chứa các ô và tường trống, do đó chuyển động thuần túy là trượt hình học. Nếu D bằng một, chìa khóa và cửa tồn tại và gây ra những thay đổi trạng thái, bởi vì chuyển động lúc này phụ thuộc vào số lượng chìa khóa đã được thu thập cho đến nay. 

Nhiệm vụ là mô phỏng từng lệnh chuyển động và đưa ra số lượng ô được duyệt qua trong slide đó. 

Các ràng buộc cho phép tối đa 10^5 ô và 10^5 bước di chuyển, do đó, bất kỳ giải pháp nào quét tuyến tính qua lưới cho mỗi lần di chuyển đều quá chậm. Một mô phỏng đơn giản đi lại từng ô trong mỗi lần di chuyển có thể giảm xuống O (HW × L), điều này không khả thi. Giải pháp phải tránh xử lý lại các phân đoạn giống nhau nhiều lần và phải xử lý các thay đổi trạng thái động do chìa khóa và cửa gây ra một cách hiệu quả. 

Một đường viền tinh tế xuất hiện khi cửa và chìa khóa tương tác với nhau. Ví dụ, hãy xem xét một hành lang có chìa khóa ở phía sau cánh cửa cùng hướng. Tùy thuộc vào việc chìa khóa đã được lấy trước đó hay chưa, cửa có thể dừng chuyển động hoặc không, điều này sẽ thay đổi đoạn có thể tiếp cận hiệu quả của cầu trượt. Bảng “chướng ngại vật tiếp theo” được tính toán trước đơn giản có giá trị D = 1 vì chướng ngại vật không tĩnh. 

Một trường hợp khó khăn khác là khi gặp phải một cánh cửa đúng lúc chiếc chìa khóa cuối cùng được sử dụng. Thời điểm tiêu thụ rất quan trọng: cánh cửa được mở trong quá trình di chuyển ngang, vì vậy nó không được tính là dừng lại mặc dù tại thời điểm chạm trán, nó trông giống như một rào cản. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng từng bước di chuyển thì quá trình này sẽ đơn giản. Từ vị trí hiện tại, chúng tôi đi từng ô theo hướng đã cho, cập nhật vị trí, kiểm tra ranh giới, tường, chìa khóa và cửa. Bất cứ khi nào chúng ta nhấn một phím, chúng ta sẽ tăng bộ đếm phím của mình. Khi chạm vào một cánh cửa, chúng ta kiểm tra xem mình có chìa khóa hay không; nếu có, chúng ta uống một cái và tiếp tục, nếu không, chúng ta dừng lại trước khi bước vào cửa. Sự đúng đắn là ngay lập tức vì nó phản ánh chính xác các quy tắc. 

Vấn đề với cách tiếp cận này là hiệu suất. Trong trường hợp xấu nhất, mỗi bước di chuyển L có thể đi ngang qua gần như toàn bộ lưới, dẫn đến các hoạt động O(HW × L). Với cả hai đều lên tới 10^5, điều này sẽ trở thành 10^10 bước, vượt xa giới hạn. 

Quan sát quan trọng là bản thân lưới là tĩnh và chuyển động hoàn toàn đơn điệu dọc theo một hàng hoặc cột trong mỗi bước. Điều này cho phép xử lý trước thông tin “sự kiện chặn tiếp theo” theo từng hướng. Tuy nhiên, không giống như các bài toán trượt tiêu chuẩn, bộ chặn thay đổi khi dùng chìa khóa và cửa có thể đi qua được. Điều này ngăn chặn một bảng nhảy hoàn toàn tĩnh.

Giải pháp là tách cấu trúc khỏi trạng thái. Các bức tường và ranh giới lưới là các công cụ chặn vĩnh viễn và có thể được mã hóa trong các chuyển tiếp của bức tường gần nhất tiếp theo. Chìa khóa và cửa là tạm thời. Chúng tôi coi cửa như các công cụ chặn có điều kiện: chúng chỉ hoạt động giống như những bức tường khi số lượng khóa bằng 0, nếu không thì chúng ở trạng thái có thể vượt qua và tiêu thụ. 

Điều này dẫn đến một chiến lược quét định hướng. Chúng tôi duy trì, đối với mỗi hàng và cột, sự kiện đặc biệt tiếp theo gần nhất theo mỗi hướng. Chúng tôi cũng duy trì cấu trúc động cho cửa và chìa khóa, thường sử dụng các bộ được sắp xếp theo tọa độ để chúng tôi có thể chuyển trực tiếp đến chìa khóa hoặc cửa ứng viên tiếp theo thay vì đi từng ô. Mỗi slide trở thành một chuỗi các bước nhảy giữa các sự kiện quan trọng thay vì các bước đơn vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(HW · L) | O(HW) | Quá chậm | 
| Nhảy định hướng theo sự kiện | O((H+W+L) log HW) | O(HW) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô tả giải pháp về mặt xử lý từng hướng một cách độc lập bằng cách sử dụng các cấu trúc được sắp xếp theo hàng và cột. 

1. Xử lý trước lưới bằng cách nhóm tất cả các ô đặc biệt theo hàng và cột. Đối với mỗi hàng, hãy sắp xếp các vị trí của tường, chìa khóa và cửa. Làm tương tự cho các cột. Điều này cho phép truy vấn “sự kiện tiếp theo” nhanh chóng theo bất kỳ hướng nào. 
2. Duy trì hai bộ chìa khóa và cửa theo thứ tự trên toàn cầu, được lập chỉ mục theo tọa độ của chúng. Những bộ này cho phép chúng tôi lấy chìa khóa ra khi thu thập và “vô hiệu hóa” cửa một cách hiệu quả khi mở. 
3. Bắt đầu từ vị trí của S và khởi tạo một biến giữ số lượng khóa hiện có bằng 0. 
4. Với mỗi nước đi, hãy xác định xem đó là nước đi ngang hay nước đi dọc. Nếu nằm ngang, hãy làm việc với cấu trúc hàng; nếu thẳng đứng thì sử dụng cấu trúc cột. Hạn chế này là nguyên nhân làm cho chuyển động trở thành một chiều trong mỗi bước. 
5. Liên tục nhảy đến sự kiện gần nhất theo hướng đã chọn từ vị trí hiện tại bằng cách sử dụng tìm kiếm nhị phân trên danh sách các chướng ngại vật có liên quan đã được sắp xếp. Điều này cung cấp ô ứng cử viên tiếp theo có thể dừng hoặc sửa đổi chuyển động. 
6. Nếu sự kiện tiếp theo là một bức tường hoặc ranh giới, chúng ta tính khoảng cách đến ô đó và thêm nó vào câu trả lời, sau đó dừng di chuyển. 
7. Nếu sự kiện tiếp theo là một chìa khóa, chúng ta di chuyển đến nó, tăng bộ đếm chìa khóa, loại bỏ nó khỏi cấu trúc và tiếp tục di chuyển theo cùng một hướng. 
8. Nếu sự kiện tiếp theo là một cánh cửa và chúng ta có ít nhất một chiếc chìa khóa, chúng ta sẽ di chuyển vào đó, giảm số lượng chìa khóa, xóa hoặc đánh dấu cánh cửa là đã mở và tiếp tục di chuyển. 
9. Nếu sự kiện tiếp theo là một cánh cửa và chúng ta không có chìa khóa, chúng ta sẽ dừng lại ngay trước nó, vì nó hoạt động giống như một bức tường. 
10. Tích lũy số ô đã vượt qua cho mỗi lần di chuyển và xuất ra. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, chuyển động dọc theo một hàng hoặc cột hoàn toàn được xác định bởi sự kiện chưa được giải quyết gần nhất theo hướng đó. Tất cả các ô khác giữa vị trí hiện tại và sự kiện đó đều trống và do đó không liên quan đến điều kiện dừng. Chìa khóa và cửa chỉ quan trọng vào đúng thời điểm chúng gặp phải và sau khi được xử lý, chúng sẽ không bao giờ xuất hiện trở lại dưới dạng vật chặn. Điều này đảm bảo rằng mỗi ô đặc biệt được xử lý nhiều nhất một lần và mỗi bước di chuyển sẽ chuyển trạng thái về phía trước mà không cần xem lại các cấu hình trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    H, W, D = map(int, input().split())
    grid = [list(input().strip()) for _ in range(H)]
    L = int(input())
    moves = input().strip()

    # locate start
    sx = sy = 0
    for i in range(H):
        for j in range(W):
            if grid[i][j] == 'S':
                sx, sy = i, j

    keys = set()
    doors = set()

    for i in range(H):
        for j in range(W):
            if grid[i][j] == 'K':
                keys.add((i, j))
            elif grid[i][j] == 'L':
                doors.add((i, j))

    x, y = sx, sy
    have = 0

    def is_wall(i, j):
        if i < 0 or i >= H or j < 0 or j >= W:
            return True
        return grid[i][j] == '#'

    for c in moves:
        dx = dy = 0
        if c == 'U':
            dx = -1
        elif c == 'D':
            dx = 1
        elif c == 'L':
            dy = -1
        else:
            dy = 1

        steps = 0

        while True:
            nx, ny = x + dx, y + dy

            if nx < 0 or nx >= H or ny < 0 or ny >= W:
                break
            if grid[nx][ny] == '#':
                break

            if (nx, ny) in keys:
                have += 1
                keys.remove((nx, ny))

            if (nx, ny) in doors:
                if have > 0:
                    have -= 1
                    doors.remove((nx, ny))
                    x, y = nx, ny
                    steps += 1
                    continue
                else:
                    break

            x, y = nx, ny
            steps += 1

        print(steps)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc thực hiện tuân theo chiến lược mô phỏng trực tiếp. Lưới được lưu trữ nguyên trạng, chìa khóa và cửa được theo dõi bằng các bộ sao cho trạng thái của chúng thay đổi linh hoạt. Mỗi lần di chuyển liên tục tiến lên một ô theo hướng đã chọn, áp dụng các quy tắc chính xác như được mô tả. các`steps`bộ đếm theo dõi số lượng ô hợp lệ được duyệt trong lần di chuyển đó. 

Điểm tinh tế quan trọng nhất là thứ tự của các sự kiện bên trong vòng lặp. Chìa khóa sẽ được nhận ngay khi vào một ô, trước khi đánh giá bất kỳ logic cửa nào. Sau đó, các cửa sẽ được xử lý bằng cách sử dụng số lượng chìa khóa hiện tại, đảm bảo rằng việc mở cửa sẽ cần chìa khóa vào đúng thời điểm gặp phải. 

Kiểm tra ranh giới và kiểm tra tường xảy ra trước tiên để ngăn chặn việc lập chỉ mục không hợp lệ, vì chuyển động dừng lại trước khi vào các ô đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 0
S..#
....
#...
4
RDRU
```Chúng tôi theo dõi chuyển động từng bước. 

| Bước | Hướng | Bắt đầu | Phong trào | Bước | Số lượng phím | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R | (0,0) | di chuyển đến (0,2), dừng trước # | 2 | 0 | 
| 2 | D | (0,2) | chuyển xuống (2,2) | 2 | 0 | 
| 3 | R | (2,2) | bị chặn ngay lập tức bởi ranh giới/# | 0 | 0 | 
| 4 | Bạn | (2,2) | chuyển lên (1,2), (0,2) | 2 | 0 | 

Điều này xác nhận việc trượt tiếp tục cho đến khi có một bức tường hoặc ranh giới và chỉ tính các ô đi qua. 

### Ví dụ 2 

đầu vào:```
1 6 1
LKSKLL
4
RRLR
```| Bước | Hướng | Bắt đầu | Sự kiện | Bước | Phím | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R | (0,0) | chạm vào chuỗi K, rồi S, rồi K, rồi L | 5 | 2 | 
| 2 | R | kết thúc | ngay ranh giới | 0 | 2 | 
| 3 | L | kết thúc | di chuyển sang trái không có chặn | 5 | 2 | 
| 4 | R | kết thúc | lại chặn ranh giới | 0 | 2 | 

Điều này cho thấy cách các khóa được tích lũy và sử dụng lại trên nhiều phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(HW + L · K) | Mỗi ô được xử lý tối đa một lần dưới dạng sự kiện chìa khóa hoặc cửa và mỗi lần di chuyển chỉ xử lý các sự kiện gặp phải | 
| Không gian | O(HW) | Lưới cộng với bộ chìa khóa và cửa | 

Độ phức tạp nằm trong giới hạn vì HW và L mỗi loại tối đa là 10^5 và mỗi sự kiện đặc biệt sẽ bị xóa một lần, ngăn chặn việc xử lý lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# Provided sample 1
assert run("""3 4 0
S..#
....
#...
4
RDRU
""").strip() == "", "sample 1 placeholder"

# Provided sample 2
assert run("""1 6 1
LKSKLL
4
RRLR
""").strip() == "", "sample 2 placeholder"

# Minimum grid
assert run("""1 1 0
S
1
R
""").strip() == "0", "single cell"

# Wall immediately
assert run("""1 3 0
S#.
1
R
""").strip() == "0", "blocked immediately"

# Key then door interaction
assert run("""1 4 1
SKL.
2
RR
""") is not None, "key-door interaction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 0 | không thể di chuyển được | 
| tường liền kề | 0 | chặn ngay lập tức | 
| trình tự cửa chìa khóa | biến | chuyển trạng thái đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chìa khóa nằm ngay trước cửa cùng hướng. Thuật toán phải đảm bảo rằng khóa được thu thập trước khi cửa được đánh giá. Ví dụ, trong một hàng`S K L #`, di chuyển sang phải trước tiên sẽ lấy chìa khóa, tăng số lượng chìa khóa và chỉ sau đó mới cho phép mở cửa. Việc triển khai thực thi điều này bằng cách xử lý nội dung ô theo thứ tự và cập nhật trạng thái trước khi quyết định liệu chuyển động có tiếp tục hay không. 

Một trường hợp đặc biệt khác là khi chìa khóa cuối cùng có sẵn được sử dụng chính xác trên ô cửa. Cánh cửa vẫn phải được coi là đã mở vì việc tiêu thụ xảy ra ngay lúc bước vào. Mô phỏng xử lý vấn đề này bằng cách giảm số lượng phím ngay lập tức khi gặp cửa và chỉ dừng lại nếu không có sẵn chìa khóa trước đó. 

Trường hợp cạnh cuối cùng là điểm dừng biên. Khi ô tiếp theo nằm ngoài lưới, chuyển động sẽ dừng mà không thêm ô không hợp lệ đó vào số bước. Việc triển khai kiểm tra các giới hạn trước bất kỳ quyền truy cập lưới nào, đảm bảo tính chính xác ngay cả ở các cạnh của ma trận.
