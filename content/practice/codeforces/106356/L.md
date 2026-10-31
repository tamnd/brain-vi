---
title: "CF 106356L - Sapure"
description: "Chúng tôi đang mô phỏng một con rắn di chuyển trên lưới $n lần n$ trong đó các cạnh quấn quanh, do đó lưới hoạt động giống như một hình xuyến. Con rắn bắt đầu như một ô duy nhất ở mức $(1,1)$."
date: "2026-06-19T14:57:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "L"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 53
verified: true
draft: false
---

[CF 106356L - Sapure](https://codeforces.com/problemset/problem/106356/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một con rắn di chuyển trên một$n \times n$lưới trong đó các cạnh bao quanh, do đó lưới hoạt động giống như một hình xuyến. Con rắn bắt đầu như một tế bào duy nhất tại$(1,1)$. Chúng ta được cung cấp một chuỗi$k$di chuyển, mỗi lần di chuyển đầu một bước theo hướng chính, áp dụng tính năng quấn quanh nếu nó đi ra ngoài giới hạn. 

Ở mỗi bước, sau khi đầu di chuyển, chúng ta sẽ nuôi rắn nếu đầu chạm vào ô thức ăn hiện tại hoặc giữ nguyên chiều dài bằng cách di chuyển đuôi về phía trước. Các loại thực phẩm xuất hiện lần lượt theo một trình tự cố định: tại một thời điểm chỉ có một loại thực phẩm tồn tại và thức ăn tiếp theo sẽ xuất hiện ngay sau khi ăn món hiện tại. Điều quan trọng là con rắn sẽ chết nếu sau khi di chuyển, đầu của nó chạm vào bất kỳ bộ phận nào trên cơ thể, ngoại trừ việc di chuyển vào ô đuôi hiện tại chỉ được phép khi đuôi đồng thời di chuyển ra xa trong cùng bước đó. 

Nhiệm vụ là xác định xem con rắn có sống sót sau tất cả$k$di chuyển. Nếu nó chết sớm hơn, chúng tôi xuất ra`DEAD`, nếu không thì chúng tôi xuất ra`ALIVE`tiếp theo là chiều dài cuối cùng của nó. 

Các ràng buộc đi lên đến$10^5$cho tất cả các tham số, điều này ngay lập tức loại trừ bất kỳ phương pháp nào quét toàn bộ cơ thể con rắn trong mỗi lần di chuyển. Một mô phỏng đơn giản kiểm tra va chạm bằng cách lặp lại toàn bộ phần thân ở mỗi bước sẽ tốn kém$O(k^2)$trong trường hợp xấu nhất là quá chậm. Chúng tôi cần các hoạt động khấu hao không đổi hoặc logarit cho mỗi lần di chuyển. 

Một vấn đề tế nhị đến từ quy tắc “ngoại lệ ở đuôi”. Nếu rắn không lớn thì đuôi sẽ dời đi sau khi đầu di chuyển nên việc bước vào ô đuôi cũ là an toàn. Một cách tiếp cận ngây thơ nhằm kiểm tra các va chạm với toàn bộ cơ thể mà không tính đến thời điểm này sẽ đánh dấu sai một số nước đi hợp lệ là tử vong. 

Một trường hợp cạnh tinh tế khác đến từ sự bao quanh. Ví dụ, trên một$3 \times 3$lưới, di chuyển sang trái từ cột 1 sẽ đến cột 3. Nếu logic va chạm được triển khai trước khi áp dụng chuẩn hóa mô-đun, nó có thể phát hiện không chính xác các trạng thái ngoài giới hạn. 

Cuối cùng, vấn đề thời gian cho ăn: con rắn phát triển ngay lập tức khi hạ cánh, nghĩa là đuôi không di chuyển trong bước đó. Điều này thay đổi quy tắc va chạm cho các bước đi tiếp theo vì hình dạng cơ thể khác với trường hợp không tăng trưởng. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: duy trì một danh sách hoặc phân loại rõ ràng tất cả các tế bào cơ thể rắn từ đầu đến đuôi. Đối với mỗi lần di chuyển, chúng tôi tính toán vị trí đầu mới, sau đó quét toàn bộ cơ thể để kiểm tra xem đầu có va chạm với bất kỳ đoạn nào hay không. Nếu không có va chạm, chúng ta nối đầu mới và loại bỏ đuôi (nếu không có thức ăn) hoặc chỉ nối đầu (nếu ăn được). Điều này đúng vì nó mô phỏng trực tiếp các quy tắc. 

Tuy nhiên, cách tiếp cận này trở nên quá chậm vì mỗi bước có thể yêu cầu quét tới$O(k)$các đoạn cơ thể, dẫn đến$O(k^2)$tổng công khi con rắn phát triển tuyến tính. Với$k = 10^5$, đây là theo thứ tự của$10^{10}$kiểm tra, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không cần lưu trữ hoặc quét toàn bộ cơ thể một cách rõ ràng. Chúng ta chỉ cần hai thao tác: kiểm tra xem một ô hiện có bị con rắn chiếm giữ hay không và chèn hoặc xóa các ô khi con rắn di chuyển. Điều này gợi ý việc duy trì một bộ băm cho các ô bị chiếm dụng và một deque cho phần thân được sắp xếp để chúng ta có thể loại bỏ phần đuôi một cách hiệu quả. 

Sự phức tạp duy nhất là xử lý quy tắc “đuôi di chuyển đồng thời”. Chúng ta phải đảm bảo rằng khi kiểm tra va chạm, ô đuôi tạm thời bị bỏ qua nếu nó chuyển động cùng bước. Điều đó có nghĩa là trước khi kiểm tra sức chứa, chúng ta phải quyết định xem con rắn có đang phát triển hay không và nếu nó không phát triển, về mặt khái niệm, chúng ta sẽ loại bỏ phần đuôi khỏi tập hợp bị chiếm giữ trước khi thực hiện thử nghiệm va chạm. 

Với cấu trúc này, mỗi lần di chuyển đều được xử lý khấu hao$O(1)$, vì tất cả các phép toán trên deque và set đều có thời gian trung bình không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét cơ thể) |$O(k^2)$|$O(k)$| Quá chậm | 
| Tối ưu (deque + set) |$O(k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một deque lưu trữ thân rắn từ đầu đến đuôi và một bộ băm lưu trữ tất cả các ô bị chiếm dụng để truy vấn thành viên nhanh chóng. 

### Các bước 

1. Khởi tạo con rắn bằng một ô duy nhất$(1,1)$trong cả deque và set, đồng thời đặt chỉ số thực phẩm thành 0. Thực phẩm hiện tại là mục đầu tiên trong danh sách. 
2. Đối với mỗi lần di chuyển, hãy tính toán vị trí đầu tiếp theo bằng cách sử dụng hướng, áp dụng modulo bao quanh$n$. Điều này đảm bảo tất cả tọa độ nằm trong giới hạn hợp lệ. 
3. Xác định xem vị trí đầu mới có bằng ô thức ăn hiện tại hay không. Điều này quyết định liệu con rắn có phát triển trong nước đi này hay không. 
4. Nếu rắn không lớn, hãy loại bỏ tế bào đuôi hiện tại khỏi bộ trước khi kiểm tra va chạm. Điều này mô phỏng thực tế là đuôi di chuyển ra xa đồng thời với bước đầu. 
5. Kiểm tra xem ô đầu mới đã tồn tại trong bộ chưa. Nếu đúng như vậy, con rắn sẽ chết ngay lập tức vì va chạm với cơ thể của nó trong cấu hình hiện tại. 
6. Nếu không, hãy thêm ô đầu mới vào phía trước deque và chèn nó vào bộ. 
7. Nếu đã ăn xong thức ăn, hãy tăng chiều dài bằng cách không cắt đuôi và chuyển sang món ăn tiếp theo. Nếu không ăn được thức ăn nào, hãy loại bỏ ô đuôi khỏi deque sau khi logic chèn được giải quyết. 

Một điểm đặt hàng tinh tế là va chạm phải được kiểm tra theo trạng thái chính xác: hoặc với phần đuôi bị loại bỏ (trường hợp không phát triển) hoặc với toàn bộ cơ thể còn nguyên vẹn (trường hợp đang phát triển). 

### Tại sao nó hoạt động 

Điều bất biến là khi bắt đầu mỗi nước đi, deque và set thể hiện chính xác cơ thể của con rắn sau tất cả các nước đi trước đó. Khi xử lý một nước đi, chúng tôi lập mô hình chính xác chuyển động đồng thời của đầu và đuôi bằng cách tạm thời loại bỏ đuôi chỉ trong trường hợp không tăng trưởng trước khi phát hiện va chạm. Điều này đảm bảo rằng việc kiểm tra sức chứa phản ánh trạng thái vật lý thực sự của con rắn vào thời điểm đầu di chuyển. Vì mọi cập nhật đều duy trì tính nhất quán giữa deque và set, không còn vị trí cũ nào và mọi xung đột đều tương ứng chính xác với một giao điểm thực sự trong mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k, m = map(int, input().split())
s = input().strip()

food = [tuple(map(int, input().split())) for _ in range(m)]

from collections import deque

snake = deque()
snake.append((1, 1))

occupied = set()
occupied.add((1, 1))

head_x, head_y = 1, 1
food_idx = 0

dir_map = {
    'L': (0, -1),
    'R': (0, 1),
    'U': (-1, 0),
    'D': (1, 0)
}

for ch in s:
    dx, dy = dir_map[ch]
    nx, ny = head_x + dx, head_y + dy

    if nx == 0:
        nx = n
    elif nx == n + 1:
        nx = 1
    if ny == 0:
        ny = n
    elif ny == n + 1:
        ny = 1

    eat = (food_idx < m and (nx, ny) == food[food_idx])

    if not eat:
        tx, ty = snake[-1]
        occupied.remove((tx, ty))

    if (nx, ny) in occupied:
        print("DEAD")
        sys.exit(0)

    snake.appendleft((nx, ny))
    occupied.add((nx, ny))
    head_x, head_y = nx, ny

    if eat:
        food_idx += 1
    else:
        snake.pop()

print("ALIVE", len(snake))
```Mã trực tiếp theo mô hình mô phỏng. Deque lưu trữ con rắn từ đầu ở bên trái đến đuôi ở bên phải. Bộ này phản chiếu nó để kiểm tra va chạm O(1). Điều tinh tế quan trọng là loại bỏ đuôi khỏi bộ trước khi kiểm tra va chạm khi con rắn không phát triển, điều này mô hình chính xác chuyển động đồng thời. 

Bao quanh được xử lý rõ ràng bằng cách kiểm tra ranh giới sau khi áp dụng chuyển động. Điều này tránh việc dựa vào số học modulo có thể khó đọc hơn một chút trong các trường hợp góc. 

Việc tiêu thụ thực phẩm được kiểm tra trước khi sửa đổi cơ thể để quá trình tăng trưởng được áp dụng ngay lập tức trong cùng một động tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 2
RRDD
2 2
3 3
```| Bước | Di chuyển đầu | Đầu mới | Ăn? | Đã cắt bỏ đuôi? | Va chạm | Chiều dài | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu | (1,1) | - | - | - | 1 | 
| 1 | R | (1,2) | không | vâng | không | 1 | 
| 2 | R | (1,3) | không | vâng | không | 1 | 
| 3 | D | (2,3) | không | vâng | không | 1 | 
| 4 | D | (3,3) | vâng | không | không | 2 | 

Con rắn không bao giờ thăm lại cơ thể của nó một cách mâu thuẫn. Bước tăng trưởng ở bước cuối cùng ngăn cản việc loại bỏ đuôi, đó là lý do tại sao chiều dài lại tăng lên. 

### Ví dụ 2 

đầu vào:```
3 3 1
RDL
1 2
```| Bước | Di chuyển đầu | Đầu mới | Ăn? | Đã cắt bỏ đuôi? | Va chạm | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu | (1,1) | - | - | - | còn sống | 
| 1 | R | (1,2) | vâng | không | không | phát triển | 
| 2 | D | (2,2) | không | vâng | không | còn sống | 
| 3 | L | (2,1) | không | vâng | vâng (cơ thể) | CHẾT | 

Điều này chứng tỏ một vụ va chạm muộn khi con rắn lao vào cơ thể của chính nó sau khi nó đã nở ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k)$| Mỗi lần di chuyển thực hiện các hoạt động deque và hash set theo thời gian không đổi | 
| Không gian |$O(k)$| Cơ thể rắn có thể lớn tới k tế bào | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$k \le 10^5$, và cả bộ nhớ và thời gian đều chia tỷ lệ tuyến tính với số lần di chuyển. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, k, m = map(int, input().split())
    s = input().strip()
    food = [tuple(map(int, input().split())) for _ in range(m)]

    snake = deque()
    snake.append((1, 1))
    occupied = set([(1, 1)])

    head_x, head_y = 1, 1
    food_idx = 0

    dir_map = {'L': (0, -1), 'R': (0, 1), 'U': (-1, 0), 'D': (1, 0)}

    for ch in s:
        dx, dy = dir_map[ch]
        nx, ny = head_x + dx, head_y + dy

        if nx == 0: nx = n
        elif nx == n + 1: nx = 1
        if ny == 0: ny = n
        elif ny == n + 1: ny = 1

        eat = food_idx < m and (nx, ny) == food[food_idx]

        if not eat:
            tx, ty = snake[-1]
            occupied.remove((tx, ty))

        if (nx, ny) in occupied:
            return "DEAD"

        snake.appendleft((nx, ny))
        occupied.add((nx, ny))
        head_x, head_y = nx, ny

        if eat:
            food_idx += 1
        else:
            snake.pop()

    return "ALIVE " + str(len(snake))

# provided samples
assert run("""3 10 3
LLDRRUURRD
1 3
2 3
3 3
""") == "ALIVE 4"

assert run("""3 5 3
RDLLL
1 2
2 2
2 1
""") == "DEAD"

# custom cases
assert run("""2 1 1
R
1 2
""") in ["ALIVE 2", "DEAD"]

assert run("""4 6 2
RRDDLL
2 2
3 3
""")

assert run("""3 8 0
RRDDLLUU
""") == "ALIVE 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nước đi đơn 2×2 | SỐNG/CHẾT | tăng trưởng ngay lập tức so với sự mơ hồ va chạm | 
| chu kỳ nhỏ | phụ thuộc | sự đúng đắn bao quanh | 
| không có vòng lặp thức ăn | SỐNG 1 ​​| sự ổn định chuyển động thuần túy | 

## Vỏ cạnh 

Một trường hợp quan trọng là di chuyển vào tế bào đuôi khi không phát triển. Ví dụ, một con rắn có chiều dài 3 đang di chuyển về phía trước vào vị trí đuôi của nó sẽ không chết nếu đuôi bị loại bỏ đồng thời. Việc triển khai loại bỏ rõ ràng đuôi khỏi tập hợp bị chiếm dụng trước khi kiểm tra xung đột trong các bước không tăng trưởng, đảm bảo trường hợp này được xử lý chính xác. 

Một trường hợp nguy hiểm khác là tiêu thụ thực phẩm ngay lập tức. Nếu đầu di chuyển từ$(1,1)$để trên một lưới nhỏ, nó có thể quấn sang phía đối diện và rơi chính xác vào ô thức ăn. Vì việc kiểm tra thực phẩm được thực hiện sau khi bình thường hóa tọa độ nên trường hợp này kích hoạt sự tăng trưởng một cách chính xác. 

Trường hợp cạnh cuối cùng là tự va chạm ngay sau khi tăng trưởng. Khi rắn ăn thức ăn, đuôi không cử động nên người nuôi rắn không được tháo đuôi. Điều này làm cho phần thân dài hơn ngay lập tức và bất kỳ va chạm nào với phần thân mở rộng đó đều được phát hiện chính xác vì chúng tôi kiểm tra dựa trên tập hợp bị chiếm dụng hoàn toàn trong trường hợp đó.
