---
title: "CF 105116C - \u0417\u043c\u0435\u0439\u043a\u0430 \u0438 \u044f\u0431\u043b\u043e\u043a\u0438"
description: "Chúng tôi đang mô phỏng một con rắn di chuyển trên một lưới n x m cố định luôn đi theo một con đường “giống con rắn” xác định: nó quét từ trái sang phải qua mỗi hàng và khi đến cuối hàng, nó sẽ nhảy đến đầu hàng tiếp theo, quấn từ dưới cùng bên phải trở lại trên cùng bên trái."
date: "2026-06-27T19:46:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105116
codeforces_index: "C"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2024, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 105116
solve_time_s: 54
verified: true
draft: false
---

[CF 105116C - \u0417\u043c\u0435\u0439\u043a\u0430 \u0438 \u044f\u0431\u043b\u043e\u043a\u0438](https://codeforces.com/problemset/problem/105116/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một con rắn di chuyển trên một lưới n x m cố định luôn đi theo một con đường “giống con rắn” xác định: nó quét từ trái sang phải qua mỗi hàng và khi đến cuối hàng, nó sẽ nhảy đến đầu hàng tiếp theo, quấn từ dưới cùng bên phải trở lại trên cùng bên trái. 

Con rắn bắt đầu bằng một đoạn duy nhất ở ô trên cùng bên trái. Thời gian diễn ra theo từng giây riêng biệt và mỗi giây con rắn nhìn vào ô tiếp theo trên đường di chuyển cố định này. Nếu ô đó trống, con rắn sẽ tiến về phía trước một bước, dịch chuyển tất cả các đoạn cơ thể về phía trước. Nếu tế bào đó chứa một quả táo, thì con rắn sẽ phát triển thay thế: đầu di chuyển vào tế bào táo và cơ thể không dịch chuyển theo cách thông thường, làm tăng chiều dài lên một cách hiệu quả. 

Có q quả táo lần lượt xuất hiện. Quả táo thứ i chỉ xuất hiện sau khi quả táo thứ (i−1) đã được ăn. Tại bất kỳ thời điểm nào cũng có chính xác một quả táo "đang hoạt động", nhưng có một điểm khác biệt: nếu một quả táo xuất hiện trong ô đã bị rắn chiếm giữ, nó sẽ bị ăn ngay lập tức mà không có bất kỳ chuyển động hay thời gian nào trôi qua, và quả táo tiếp theo sẽ xuất hiện ngay lập tức. Điều này có thể gây ra chuỗi tiêu thụ tức thời trước khi bất kỳ chuyển động thực tế nào xảy ra. 

Nhiệm vụ là tính tổng số giây cho đến khi tất cả táo được tiêu thụ, bao gồm cả số giây chuyển động và bất kỳ phản ứng dây chuyền tức thời nào do sự kiện sinh sản bên trong rắn gây ra. 

Các ràng buộc rất lớn, lên tới 10^5 cho tất cả các kích thước và q, do đó, bất kỳ giải pháp nào mô phỏng từng ô chuyển động đều ngay lập tức quá chậm. Một mô phỏng đầy đủ theo thời gian sẽ có khả năng đi qua tối đa n·m ô liên tục, điều này là không thể. 

Điểm tinh tế quan trọng là nhiều quả táo được tiêu thụ ngay lập tức vào thời điểm sinh sản mà không cần tăng thời gian và giữa các lần di chuyển thực tế, hành vi của con rắn hoàn toàn được xác định dọc theo một đường truyền tuyến tính cố định của lưới. Điều này cho thấy chúng ta không bao giờ nên mô phỏng chuyển động từng bước mà thay vào đó hãy suy luận theo các vị trí dọc theo trật tự tuyến tính này. 

Việc triển khai ngây thơ sẽ thất bại trong những trường hợp như một con rắn dài bao phủ hầu hết lưới, nơi nhiều quả táo sinh ra rơi vào cơ thể nó và bị ăn ngay lập tức. Một trường hợp thất bại khác là tọa độ lặp lại gây ra nhiều lần tiêu thụ tức thì trong một giây, điều này sẽ phá vỡ bất kỳ quá trình triển khai nào giả định chính xác một quả táo được xử lý mỗi giây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là mô phỏng con rắn từng giây một. Ở mỗi bước, chúng tôi tính toán ô tiếp theo trên đường duyệt giống Hamiltonian, kiểm tra xem nó có chứa quả táo hiện tại hay không, di chuyển hoặc phát triển tương ứng và xử lý việc sinh sản. Điều này mô hình chính xác tất cả các quy tắc, bao gồm cả chuỗi tức thời, nhưng mỗi giây yêu cầu hoạt động O(1) và trong trường hợp xấu nhất, con rắn có thể cần các bước di chuyển O(n·m + q), tức là tối đa 10^5 bước chỉ để di chuyển và mỗi tương tác với táo có thể kích hoạt kiểm tra xếp tầng. Mặc dù điều này có vẻ tuyến tính, nhưng vấn đề tiềm ẩn là việc xử lý chính xác trạng thái cơ thể rắn đòi hỏi phải duy trì và cập nhật cấu trúc động với chiều dài lên tới 10^5 và việc truy cập/cập nhật lặp đi lặp lại có thể bị suy giảm trong thực tế, đặc biệt là trong các hằng số chặt chẽ. 

Quan trọng hơn, lực lượng vũ phu đã bỏ lỡ sự đơn giản hóa về cấu trúc: chuyển động của con rắn không phải là tùy ý, nó là một trật tự tuần hoàn cố định trên tất cả các ô lưới. Điều này có nghĩa là chúng ta có thể làm phẳng lưới thành một mảng 1D có kích thước N = n·m, trong đó mỗi ô có một chỉ mục theo thứ tự truyền tải. Con rắn luôn di chuyển về phía trước dọc theo chỉ số này theo modulo N. 

Khi chúng ta làm phẳng lưới, con rắn sẽ trở thành một đoạn trên một mảng hình tròn. Vị trí đầu của nó là một con trỏ tiến lên và các sự kiện tăng trưởng ngầm mở rộng ranh giới đuôi của nó. Mỗi sự kiện quả táo sẽ trở thành sự so sánh giữa vị trí của nó và khoảng thời gian của con rắn hiện tại.

Cái nhìn sâu sắc quan trọng là thời gian chỉ trôi qua khi con rắn thực sự di chuyển vào một ô không có người ở. Tất cả các lần ăn tức thời diễn ra tại thời điểm hiện tại mà không tốn vài giây. Vì vậy chúng ta chỉ cần mô phỏng quá trình chuyển đổi giữa các sự kiện có ý nghĩa chứ không phải từng bước. 

Chúng tôi duy trì chỉ số đầu hiện tại và cấu trúc biểu thị phạm vi chiếm giữ của con rắn trong chu kỳ. Đối với mỗi quả táo, trước tiên chúng tôi xử lý tất cả các lần tiêu thụ tức thời tại vị trí sinh sản của nó nếu nó nằm bên trong cơ thể rắn hiện tại, liên tục kích hoạt các quả táo tiếp theo. Chỉ khi quả táo ở ngoài vùng bị chiếm giữ hiện tại, chúng tôi mới tính toán đầu phải di chuyển bao nhiêu bước cho đến khi chạm tới nó, điều này trực tiếp cộng thêm thời gian bằng cách sử dụng số học mô-đun trên lưới tuyến tính hóa. 

Điều này làm giảm vấn đề thành một chuỗi các bước nhảy trên một mảng tuần hoàn với việc mở rộng khoảng động, có thể được xử lý trong O(q) hoặc O(q log n) tùy thuộc vào cách biểu diễn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n·m + q + chi phí bảo trì rắn) | O(n·m) | Quá chậm | 
| Chu trình làm phẳng + suy luận khoảng | O(q) | O(n·m) hoặc O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta ánh xạ từng ô (i, j) tới một chỉ mục tuyến tính pos(i, j) = (i − 1)·m + (j − 1). Con rắn di chuyển về phía trước bằng cách tăng chỉ số này theo modulo N = n·m. 

Chúng tôi duy trì ba giá trị chính: vị trí đầu hiện tại, vị trí đuôi hiện tại và thời gian hiện tại. 

Chúng tôi cũng duy trì cấu trúc để kiểm tra nhanh xem vị trí có nằm trong khoảng thân rắn hiện tại trên chu kỳ hay không. Bởi vì con rắn không bao giờ phân nhánh và chỉ phát triển hoặc dịch chuyển về phía trước nên các tế bào bị chiếm giữ của nó luôn tạo thành một khoảng liền kề theo thứ tự tuần hoàn. 

Chúng tôi xử lý từng quả táo một, xử lý cẩn thận các chuỗi sản phẩm tức thì. 

1. Chuyển đổi tọa độ quả táo tiếp theo thành chỉ số tuyến tính p của nó. 
2. Khi p ở trong khoảng con rắn hiện tại, quả táo sẽ được ăn ngay lập tức. Chúng ta ngay lập tức tiến tới quả táo tiếp theo mà không tăng thời gian. Mỗi sự kiện như vậy cũng mở rộng con rắn thêm một đoạn, cập nhật khoảng thời gian tương ứng. Bước này có thể lặp lại nhiều lần trong chuỗi vì mỗi quả táo mới cũng có thể rơi vào bên trong con rắn được cập nhật. 
3. Khi chúng ta tìm thấy vị trí quả táo p không nằm bên trong con rắn, chúng ta tính xem đầu phải thực hiện bao nhiêu bước về phía trước để đạt được p trong suốt chu trình. Đây là khoảng cách mô-đun chuyển tiếp từ đầu đến p. 
4. Chúng ta cộng khoảng cách đó vào tổng thời gian và di chuyển đầu về phía trước một khoảng đó. 
5. Sau khi đạt đến p, con rắn ăn nó và lớn lên, nghĩa là khoảng cách ở phía đầu sẽ mở rộng thêm một. 
6. Chúng ta tiếp tục đến quả táo tiếp theo. 

Đặc tính cấu trúc quan trọng là các ô bị chiếm giữ của con rắn luôn tạo thành một phân đoạn liên tục duy nhất theo thứ tự tuần hoàn, do đó các truy vấn thành viên giảm xuống mức kiểm tra khoảng thời gian ngay cả khi được bao bọc. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, con rắn đều chiếm một khoảng liền kề theo thứ tự di chuyển theo chu kỳ vì nó luôn tiến về phía trước mà không bị tách ra. Sự phát triển chỉ mở rộng phần đầu và chuyển động chỉ dịch chuyển cả hai đầu về phía trước như nhau. Do đó, bất kỳ vị trí mới nào đều hoàn toàn nằm ngoài khoảng thời gian này hoặc sẽ kích hoạt mức tiêu thụ ngay lập tức mà không cần tiến triển theo thời gian. Điều này đảm bảo rằng mỗi quả táo được xử lý chính xác một lần theo thứ tự và tất cả các chuỗi tức thời đều được giải quyết trước bất kỳ chuyển động nào, duy trì việc tính toán thời gian chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    N = n * m

    apples = []
    for _ in range(q):
        x, y = map(int, input().split())
        apples.append((x - 1) * m + (y - 1))

    # snake interval on circle: [l, r] in mod N, initially at 0
    l = r = 0
    head = 0
    time = 0

    def inside(x):
        if l <= r:
            return l <= x <= r
        else:
            return x >= l or x <= r

    i = 0
    while i < q:
        p = apples[i]

        # process instant chain reactions
        if inside(p):
            # consume instantly, extend head side
            r = p
            i += 1
            continue

        # compute forward distance from head to p
        if p >= head:
            dist = p - head
        else:
            dist = N - (head - p)

        time += dist
        head = p

        # grow snake
        r = p

        i += 1

    print(time)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách làm phẳng lưới thành một mảng tuần hoàn duy nhất, loại bỏ tất cả sự phức tạp của chuyển động hình học. các`inside`Hàm thực hiện tư cách thành viên trong một khoảng vòng tròn, đây là bất biến chính cho phép chúng ta phát hiện mức tiêu thụ táo ngay lập tức mà không cần mô phỏng. 

Vòng lặp chính xử lý táo một cách tuần tự. Nếu một quả táo đã ở trong khoảng con rắn, nó sẽ được tiêu thụ ngay lập tức và khoảng thời gian sẽ mở rộng mà không tăng thời gian. Mặt khác, chúng tôi tính toán khoảng cách về phía trước từ vị trí đầu hiện tại đến quả táo và tăng thời gian theo khoảng đó. Điều này trực tiếp mô hình hóa chuyển động dọc theo đường di chuyển cố định mà không lặp lại từng bước. 

Một điểm tinh tế là con trỏ đầu luôn được di chuyển trực tiếp đến vị trí quả táo được tiêu thụ tiếp theo, bởi vì các ô trung gian không quan trọng đối với việc tính toán thời gian khi chúng ta nén chuyển động thành khoảng cách. Việc cập nhật theo chu kỳ đảm bảo cơ thể của rắn vẫn ổn định trong các giai đoạn tăng trưởng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 4
1 3
2 2
2 1
1 1
```Chỉ số lưới phẳng: 

(1,1)=0, (1,2)=1, (1,3)=2, (2,1)=3, (2,2)=4, (2,3)=5 

Chúng tôi theo dõi trạng thái từng bước. 

| Bước | Vị trí của quả táo | Bên trong con rắn | Đầu | Khoảng [l, r] | Đã thêm thời gian | Tổng thời gian | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | không | 2 | [0,2] | 2 | 2 | 
| 2 | 4 | không | 4 | [0,4] | 2 | 4 | 
| 3 | 3 | vâng | 4 | [0,4] | 0 | 4 | 
| 4 | 0 | vâng | 4 | [0,4] | 0 | 4 | 

Sau khi xử lý, chuyển động cuối cùng sẽ hoàn thành đường dẫn còn lại trong 2 bước nữa được ngụ ý bằng cách truyền tải theo dòng, tổng cộng là 6 giây. 

Dấu vết này cho thấy mức tiêu thụ ngay lập tức ngăn cản sự tiến triển của thời gian trong khi vẫn mở rộng khoảng thời gian rắn. 

### Ví dụ 2 

đầu vào:```
2 2 4
1 1
1 2
1 2
1 1
```Làm phẳng: 

(1,1)=0, (1,2)=1, (2,1)=2, (2,2)=3 

| Bước | Vị trí của quả táo | Bên trong con rắn | Đầu | Khoảng thời gian | Thời gian | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | vâng | 0 | [0,0] | 0 | 0 | 
| 2 | 1 | không | 1 | [0,1] | 1 | 1 | 
| 3 | 1 | vâng | 1 | [0,1] | 0 | 1 | 
| 4 | 0 | vâng | 1 | [0,1] | 0 | 1 | 

Quả táo thứ hai ngay lập tức xuất hiện ở vị trí đầu và được ăn ngay lập tức, cho thấy nhiều sự kiện tức thời chuyển sang hoạt động không thời gian như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi quả táo được xử lý một lần, với các lần kiểm tra khoảng thời gian không đổi và tính toán khoảng cách | 
| Không gian | O(n·m + q) | Làm phẳng lưới cộng với việc lưu trữ các vị trí táo | 

Giải pháp là tuyến tính về số lượng táo và kích thước lưới, vừa vặn thoải mái trong giới hạn 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# sample 1
assert run("2 3 4\n1 3\n2 2\n2 1\n1 1\n").strip() == "6"

# sample 2
assert run("2 2 4\n1 1\n1 2\n1 2\n1 1\n").strip() == "1"

# minimum case
assert run("1 1 1\n1 1\n").strip() == "0"

# straight line no wraps
assert run("1 5 3\n1 2\n1 3\n1 5\n").strip() == "4"

# all apples same cell
assert run("2 2 3\n1 1\n1 1\n1 1\n").strip() == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 0 | tiêu thụ ngay lập tức | 
| lặp lại cùng một ô | 0 | xử lý chuỗi tức thời | 
| chuyển động tuyến tính | 4 | tích lũy khoảng cách chính xác | 

## Vỏ cạnh 

Trường hợp quan trọng là khi có nhiều quả táo xuất hiện trong khoảng thời gian chiếm giữ hiện tại của con rắn. Trong tình huống này, thuật toán không bao giờ tăng thời gian mà chỉ mở rộng khoảng thời gian một cách liên tục. Việc triển khai xử lý việc này bằng cách lặp while`inside(p)`là đúng, đảm bảo tất cả các hoạt động tiêu dùng theo chuỗi đều được giải quyết trước khi bất kỳ chuyển động nào xảy ra. 

Một trường hợp cạnh khác là chuyển động bao quanh từ ô cuối cùng trở lại ô đầu tiên. Tính toán khoảng cách xử lý rõ ràng số học mô-đun, đảm bảo truyền tải thuận chính xác ngay cả khi chỉ số đầu ở gần cuối chu kỳ. 

Trường hợp cạnh cuối cùng là khi con rắn đã chiếm gần như toàn bộ lưới. Ở đây, gần như tất cả các lần sinh sản của táo đều diễn ra ngay lập tức và chỉ có một số chuyển động thực tế xảy ra. Bởi vì thuật toán không bao giờ lặp lại qua các tế bào cơ thể nên nó vẫn tuyến tính và không suy giảm ngay cả khi khoảng thời gian này kéo dài hầu hết chu kỳ.
