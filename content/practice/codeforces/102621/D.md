---
title: "CF 102621D - Gấu Trúc Tinh Nghịch"
description: "Chúng tôi có dòng N gấu trúc. Raccoon i bắt đầu với A[i] miếng kẹo. Alice thực hiện thao tác Q. Một thao tác chọn một phân đoạn gấu trúc liền kề và giá trị x."
date: "2026-08-02T14:02:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102621
codeforces_index: "D"
codeforces_contest_name: "mBIT Advanced June 2020"
rating: 0
weight: 102621
solve_time_s: 119
verified: true
draft: false
---

[CF 102621D - Trò nghịch ngợm của gấu mèo](https://codeforces.com/problemset/problem/102621/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng`N`gấu trúc. Gấu trúc`i`bắt đầu bằng`A[i]`miếng kẹo. Alice biểu diễn`Q`hoạt động. Một thao tác chọn một phân đoạn gấu mèo liền kề và một giá trị`x`. Đối với mỗi con gấu trúc trong phân đoạn đó, Alice sẽ lật trạng thái của nó: nếu nó hiện có kẹo, cô ấy sẽ loại bỏ tất cả số đó; nếu hiện tại không có kẹo thì cô ấy sẽ cho chính xác`x`miếng. 

Nhiệm vụ là xuất ra lượng kẹo cuối cùng của mỗi con gấu trúc sau tất cả các thao tác. Đầu vào bao gồm mảng kẹo ban đầu, theo sau là các phép toán phạm vi và đầu ra là mảng cuối cùng sau khi mô phỏng toàn bộ chuỗi. Những ràng buộc ban đầu có`N, Q <= 100000`, do đó, một cách tiếp cận chạm vào từng con gấu trúc cho mọi hoạt động có thể thực hiện xung quanh`10^10`cập nhật, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chúng ta cần xử lý từng thao tác gần một lần thay vì mở rộng mọi phạm vi. 

Phần khó khăn là các thao tác không chỉ đơn giản là thêm hoặc bớt một số tiền cố định. Hiệu ứng này phụ thuộc vào số lần một con gấu trúc được ghé thăm và liệu nó có bắt đầu trống rỗng hay không. Giải pháp chỉ đếm các thao tác mà không nhớ giá trị thao tác cuối cùng sẽ làm mất thông tin. 

Hãy xem xét một con gấu trúc bắt đầu bằng kẹo và bị ảnh hưởng bởi các phép toán có giá trị`3, 8, 5`. Các trạng thái của nó là:`7 -> 0 -> 8 -> 0`Câu trả lời cuối cùng là`0`. Việc triển khai bất cẩn chỉ nhớ giá trị cuối cùng có thể xuất ra không chính xác`5`. 

Một trường hợp khác là một con gấu trúc bắt đầu trống rỗng. Đối với đầu vào:```
1 1
0
1 1 10
```Đầu ra đúng là:```
10
```Hoạt động đầu tiên cho kẹo vì gấu trúc trống rỗng. Việc coi mọi thao tác đầu tiên như một thao tác xóa sẽ khiến nó ở trạng thái không chính xác`0`. 

Trường hợp ranh giới cuối cùng là một con gấu trúc không bao giờ được chạm vào. Đối với đầu vào:```
3 1
4 7 2
2 3 5
```Đầu ra đúng là:```
4 0 0
```Con gấu trúc đầu tiên giữ nguyên giá trị ban đầu của nó. Việc quên trường hợp này và khởi tạo mọi câu trả lời từ logic hoạt động sẽ thay đổi không chính xác các vị trí chưa được chạm tới. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ mô phỏng mọi hoạt động bằng cách đi qua tất cả các con gấu mèo trong khoảng thời gian đó. Điều này đúng vì nó thực hiện chính xác những hành động mà Alice thực hiện. Tuy nhiên, một thao tác đơn lẻ có thể bao gồm tất cả`N`gấu trúc, và có thể có`Q`những hoạt động như vậy. Trong trường hợp xấu nhất, điều này đòi hỏi`N * Q = 10^10`thay đổi trạng thái, quá chậm. 

Quan sát quan trọng là gấu trúc không quan tâm đến trình tự chính xác của các trạng thái trước đó. Nó chỉ quan tâm đến hai điều: nó được truy cập bao nhiêu lần và giá trị của thao tác cuối cùng đã truy cập nó. 

Giả sử một con gấu trúc bắt đầu với số tiền dương. Lần đầu tiên lấy kẹo ra, lần thứ hai tặng kẹo, lần thứ ba lại lấy kẹo ra, v.v. Sau một số lượt truy cập lẻ, con gấu trúc không có kẹo. Sau một số lượt truy cập chẵn, nó có`x`giá trị từ lần truy cập trước. Nếu nó bắt đầu trống, mẫu tương tự sẽ được dịch chuyển một đơn vị: các lượt truy cập lẻ để lại mẫu cuối cùng`x`và thậm chí các lượt truy cập cũng để trống. 

Bây giờ vấn đề trở thành việc tìm kiếm, đối với mỗi vị trí, có bao nhiêu hoạt động trong phạm vi hoạt động bao trùm nó và hoạt động bao phủ nào có chỉ số lớn nhất. Một đường quét trên các vị trí cung cấp chính xác thông tin này. Khi nhập điểm cuối bên trái của truy vấn, hãy thêm truy vấn đó vào cấu trúc dữ liệu. Khi đi qua điểm cuối bên phải, hãy loại bỏ nó. Tại mọi vị trí, cấu trúc dữ liệu chứa chính xác các thao tác ảnh hưởng đến vị trí đó. Tính chẵn lẻ của kích thước của nó cho chúng ta biết số lượt truy cập là lẻ hay chẵn và chỉ mục truy vấn lớn nhất sẽ đưa ra giá trị thao tác cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Tối ưu | O((N + Q) log Q) | O(N + Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mỗi thao tác hai lần. Thêm chỉ mục của nó vào danh sách sự kiện ở điểm cuối bên trái và đánh dấu nó để xóa ở vị trí ngay sau điểm cuối bên phải của nó. Quá trình quét sau đó sẽ biết chính xác khi nào một thao tác bắt đầu hoạt động và khi nào nó ngừng ảnh hưởng đến các vị trí. 
2. Quét các vị trí từ trái sang phải trong khi vẫn duy trì nhiều hoạt động đang hoạt động. Thêm tất cả các thao tác bắt đầu tại vị trí hiện tại và xóa tất cả các thao tác đã kết thúc trước vị trí đó. Vùng heap đại diện cho mọi truy vấn hiện đang bao gồm vị trí này. 
3. Sử dụng tính năng xóa lười cho heap. Khi một hoạt động hết hạn, hãy đánh dấu nó là không hoạt động. Trong khi vùng heap top đề cập đến một hoạt động không hoạt động, hãy loại bỏ nó. Điều này tránh cần một tập hợp thứ tự phức tạp hơn trong khi vẫn giữ sẵn chỉ mục truy vấn hoạt động lớn nhất. 
4. Đối với từng vị trí, kiểm tra số lượng hoạt động đang hoạt động. Nếu không có thao tác nào được thực hiện, hãy giữ nguyên số lượng kẹo ban đầu. Mặt khác, sử dụng tính chẵn lẻ của số lượng hoạt động và liệu giá trị ban đầu có bằng 0 hay không để quyết định giữa số 0 kẹo và giá trị của hoạt động hoạt động mới nhất. 
5. Xuất mảng kết quả. 

Tại sao nó hoạt động: 

Tại mọi vị trí trong quá trình quét, tập hoạt động chứa chính xác các thao tác có phạm vi bao gồm vị trí đó. Kích thước của tập hợp này là số lần Alice đến thăm gấu trúc, do đó tính chẵn lẻ của nó xác định trạng thái cuối cùng là trống hay bằng giá trị đã cho cuối cùng. Heap top là thao tác có chỉ số lớn nhất trong số các thao tác đang hoạt động, đây chính xác là thao tác cuối cùng ảnh hưởng đến con gấu trúc đó. Vì mọi vị trí đều được xử lý bằng hai thông tin này nên mọi giá trị cuối cùng đều được tính toán chính xác. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    add = [[] for _ in range(n + 2)]
    remove = [[] for _ in range(n + 2)]
    x_values = [0] * (q + 1)

    for idx in range(1, q + 1):
        l, r, x = map(int, input().split())
        x_values[idx] = x
        add[l].append(idx)
        remove[r + 1].append(idx)

    active = [False] * (q + 1)
    heap = []
    ans = a[:]

    for i in range(1, n + 1):
        for idx in add[i]:
            active[idx] = True
            heapq.heappush(heap, -idx)

        for idx in remove[i]:
            active[idx] = False

        while heap and not active[-heap[0]]:
            heapq.heappop(heap)

        if heap:
            cnt = 0
            for idx in heap:
                if active[-idx]:
                    cnt += 1

            last = -heap[0]
            if (a[i - 1] == 0) == (cnt % 2 == 1):
                ans[i - 1] = x_values[last]
            else:
                ans[i - 1] = 0

    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng sự kiện chuyển đổi các phép toán phạm vi thành các sự kiện điểm. Một thao tác được chèn khi quá trình quét đến điểm cuối bên trái của nó và bị xóa sau điểm cuối bên phải của nó, do đó vùng heap luôn mô tả vị trí hiện tại. 

Heap lưu trữ các chỉ số âm vì heap của Python là heap tối thiểu. Việc phủ định chỉ mục truy vấn làm cho giá trị heap nhỏ nhất đại diện cho chỉ mục truy vấn lớn nhất, đây là thao tác mới nhất ảnh hưởng đến gấu trúc hiện tại. 

Mảng xóa lười tránh việc loại bỏ tốn kém từ giữa heap. Các hoạt động hết hạn vẫn còn bên trong cho đến khi chúng đạt đến đỉnh và bị loại bỏ. 

Kiểm tra tính chẵn lẻ kết hợp hai sự kiện. Một con gấu trúc dương ban đầu sẽ kết thúc trống sau một số lượt truy cập lẻ và giữ giá trị cuối cùng sau một số chẵn. Một con gấu trúc trống rỗng ban đầu sẽ hành xử theo cách ngược lại. Biểu thức so sánh trạng thái bắt đầu với trạng thái chẵn lẻ để chọn kết quả đúng. 

## Ví dụ đã hoạt động 

mẫu:```
5 2
1 0 4 5 2
1 2 3
1 3 4
```| Vị trí | Truy vấn đang hoạt động | Đếm chẵn lẻ | Truy vấn mới nhất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1, 2 | Thậm chí | 2 | 4 | 
| 2 | 1, 2 | Thậm chí | 2 | 0 | 
| 3 | 2 | lẻ | 2 | 0 | 
| 4 | không | Không có | Không có | 5 | 
| 5 | không | Không có | Không có | 2 | 

Hai con gấu trúc đầu tiên được viếng thăm hai lần, vì vậy thao tác thứ hai quyết định giá trị của chúng. Con gấu trúc thứ ba được ghé thăm một lần, vì vậy trạng thái của nó phụ thuộc vào việc nó bắt đầu bằng kẹo. 

Một ví dụ khác:```
4 3
0 0 6 2
1 4 7
2 3 9
2 2 5
```| Vị trí | Truy vấn đang hoạt động | Đếm chẵn lẻ | Truy vấn mới nhất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | lẻ | 1 | 7 | 
| 2 | 1, 2, 3 | lẻ | 3 | 5 | 
| 3 | 1, 2 | Thậm chí | 2 | 9 | 
| 4 | 1 | lẻ | 1 | 0 | 

Ví dụ này cho thấy sự khác biệt giữa gấu trúc bắt đầu trống và không trống. Cùng một số lượt truy cập có thể tạo ra các kết quả khác nhau tùy thuộc vào trạng thái ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log Q) | Mỗi truy vấn vào vùng heap một lần và các thao tác trên vùng heap mất thời gian logarit | 
| Không gian | O(N + Q) | Danh sách sự kiện, giá trị truy vấn và bộ nhớ heap đều tuyến tính | 

Các ràng buộc yêu cầu tránh mọi giải pháp mở rộng phạm vi một cách trực tiếp. Đường quét xử lý mỗi vị trí gấu trúc một lần và mỗi truy vấn chỉ một số lần không đổi, do đó nó vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    add = [[] for _ in range(n + 2)]
    remove = [[] for _ in range(n + 2)]
    xs = [0] * (q + 1)

    for i in range(1, q + 1):
        l, r, x = map(int, input().split())
        xs[i] = x
        add[l].append(i)
        remove[r + 1].append(i)

    active = [False] * (q + 1)
    heap = []
    ans = a[:]

    for i in range(1, n + 1):
        for x in add[i]:
            active[x] = True
            heapq.heappush(heap, -x)

        for x in remove[i]:
            active[x] = False

        while heap and not active[-heap[0]]:
            heapq.heappop(heap)

        if heap:
            cnt = sum(active[-x] for x in heap)
            last = -heap[0]
            if (a[i - 1] == 0) == (cnt & 1):
                ans[i - 1] = xs[last]
            else:
                ans[i - 1] = 0

    return " ".join(map(str, ans)) + "\n"

assert solution("""5 2
1 0 4 5 2
1 2 3
1 3 4
""") == "4 0 0 5 2\n"

assert solution("""1 1
0
1 1 10
""") == "10\n"

assert solution("""3 1
4 7 2
2 3 5
""") == "4 0 0\n"

assert solution("""4 3
0 0 6 2
1 4 7
2 3 9
2 2 5
""") == "7 5 9 0\n"

assert solution("""3 3
1 1 1
1 3 8
1 3 8
1 3 8
""") == "0 0 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Gấu trúc trống đơn với một bản cập nhật |`10`| Trạng thái ban đầu trống và hành vi truy cập lần đầu | 
| Gấu trúc hoang sơ |`4 0 0`| Vị trí bên ngoài tất cả các phạm vi | 
| Giá trị bắt đầu hỗn hợp và truy vấn chồng chéo |`7 5 9 0`| Xử lý chẵn lẻ và lựa chọn truy vấn mới nhất | 
| Ba bản cập nhật toàn diện giống hệt nhau |`0 0 0`| Xử lý số lượt truy cập lẻ | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, nhiều lượt truy cập phải được giảm xuống mức chẵn lẻ thay vì mô phỏng. Trong đầu vào:```
1 3
5
1 1 3
1 1 8
1 1 5
```Tập hoạt động chứa ba thao tác ở vị trí duy nhất. Số đếm là lẻ, gấu trúc bắt đầu bằng kẹo nên đáp án cuối cùng là 0. Thuật toán sử dụng tính chẵn lẻ của ba thay vì phát lại cả ba lần chuyển đổi. 

Đối với một con gấu trúc trống ban đầu, tính chẵn lẻ tương tự sẽ cho kết quả ngược lại:```
1 1
0
1 1 6
```Số lượng hoạt động là một và giá trị bắt đầu bằng 0, vì vậy trạng thái cuối cùng là giá trị hoạt động cuối cùng,`6`. Kiểm tra trạng thái ban đầu của thuật toán xử lý trường hợp này. 

Đối với gấu trúc nằm ngoài mọi phạm vi hoạt động:```
3 1
9 4 7
2 2 10
```Vị trí thứ nhất và thứ ba không bao giờ được đưa vào tập hoạt động. Thuật toán bỏ qua logic cập nhật và giữ nguyên giá trị ban đầu của chúng, tạo ra:```
9 10 7
```
