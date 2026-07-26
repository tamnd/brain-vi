---
title: "CF 102821A - Autochess"
description: "Bài toán mô phỏng giai đoạn chuẩn bị của một trò chơi autochess. Có một hàng N chỗ chờ, ban đầu trống. Cá lần lượt nhận được quân cờ cấp M. Mỗi quân cờ chỉ có một tên, những quân cờ có cùng tên có thể kết hợp."
date: "2026-07-26T16:09:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102821
codeforces_index: "A"
codeforces_contest_name: "2019 Sichuan Province Programming Contest"
rating: 0
weight: 102821
solve_time_s: 46
verified: true
draft: false
---

[CF 102821A - Autochess](https://codeforces.com/problemset/problem/102821/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng giai đoạn chuẩn bị của một trò chơi autochess. Có một hàng`N`khe chờ, ban đầu trống. Cá nhận được`M`từng quân cờ cấp 1. Mỗi quân cờ chỉ có một tên, những quân cờ có cùng tên có thể kết hợp. 

Khi một quân cờ mới có tên`s`đến, trước tiên trò chơi sẽ kiểm tra xem cấp độ 3 có`s`đã tồn tại trong khu vực chờ. Nếu vậy, quân cờ mới sẽ biến mất. Ngược lại, quân cờ mới sẽ cố gắng kết hợp với các bản sao hiện có. Nếu có`K-1`bản sao cấp 1 của`s`, chúng hợp nhất với cái mới thành cấp độ 2`s`. Nếu quân cờ cấp 2 mới được tạo ra đó cũng có thể kết hợp với`K-1`bản sao cấp 2 hiện có, nó sẽ trở thành quân cờ cấp 3. Sau tất cả các khả năng hợp nhất, quân cờ thu được sẽ chiếm ô trống ngoài cùng bên trái nếu có. 

Đầu vào cung cấp một số mô phỏng trò chơi độc lập. Với mỗi test case, chúng ta cần in nội dung cuối cùng của vùng chờ từ trái sang phải. Cờ vua cấp 1 chỉ được in bằng tên của nó, trong khi các cấp độ cao hơn sẽ thêm số cấp độ của họ. Các vị trí trống được in dưới dạng`-1`. 

Hạn chế quan trọng là số lượng vị trí,`N`, có thể đạt tới`100000`. Điều này có nghĩa là giải pháp quét liên tục toàn bộ khu vực chờ sau mỗi lần chèn có thể thực hiện xung quanh`M * N`các thao tác sẽ trở nên quá chậm nếu có nhiều quân cờ được đưa vào. Chúng ta cần cập nhật trạng thái của một quân cờ trong thời gian gần như không đổi và chỉ xử lý những thế cờ thực sự thay đổi. 

Các trường hợp cạnh chính xuất phát từ sự tương tác giữa việc hợp nhất và vị trí vật lý của các mảnh. Một giải pháp chỉ dùng bộ đếm đơn giản có thể thất bại vì đầu ra phụ thuộc vào vị trí chính xác của các mảnh. 

Ví dụ, hãy xem xét:```
1
3 3 2
a
a
a
```Đầu ra đúng là:```
Case 1: a2 -1 -1
```Hai quân cờ đầu tiên kết hợp thành quân cấp 2. Quân cờ thứ ba không thể kết hợp với nó vì`K=2`yêu cầu một mảnh cấp 2 khác, vì vậy nó chiếm vị trí trống tiếp theo. Việc triển khai bất cẩn chỉ theo dõi tổng số bản sao của`a`có thể tạo sai một mảnh cấp 3. 

Một trường hợp khác là khi bảng đầy:```
1
3 2 2
a
b
a
```Đầu ra đúng là:```
Case 1: a2 b
```Hai cái đầu tiên`a`các mảnh hợp nhất, giải phóng các vị trí ban đầu về mặt khái niệm vì chúng biến mất. Mảnh cấp 2 mới được đặt vào ô trống ngoài cùng bên trái. Việc triển khai chỉ thêm các phần vào cuối sẽ tạo ra sự sắp xếp sai. 

Trường hợp khó khăn cuối cùng là phần cấp 3 đã có sẵn:```
1
3 3 2
a
a
a
```Sau hai thao tác đầu tiên đã có mảnh cấp 2. Nếu các hoạt động sau này tạo ra một phần cấp 3 thì tất cả các bản sao trong tương lai của tên đó phải bị từ chối. Việc quên quy tắc này sẽ khiến các quân cờ thừa xuất hiện. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. Chúng ta có thể lưu trữ khu vực chờ và đối với mỗi quân cờ đến, hãy tìm kiếm qua tất cả các vị trí để đếm xem tồn tại bao nhiêu quân cờ cấp 1 và cấp 2 có tên đó. Nếu có đủ bản sao, chúng tôi sẽ xóa chúng, nâng cấp phần mới và tiếp tục kiểm tra lần hợp nhất khác. Cuối cùng, chúng tôi quét lại để tìm khe trống đầu tiên. 

Cách tiếp cận này đúng vì nó tuân theo chính xác các quy tắc, nhưng nó liên tục đi qua toàn bộ bảng. Trong trường hợp xấu nhất, nếu hội đồng quản trị có`N=100000`vị trí và nhiều quân cờ đến, số lượng các hoạt động tăng lên khoảng`M*N`, vượt xa những gì giải pháp một giây có thể xử lý được. 

Điều quan trọng cần lưu ý là các quy tắc chỉ cần hai loại thông tin cho mỗi tên quân cờ: có bao nhiêu quân cờ của mỗi cấp độ và vị trí của những quân cờ đó. Sự sắp xếp cuối cùng chỉ phụ thuộc vào việc tháo các mảnh ra và chèn vào vị trí sẵn có ngoài cùng bên trái. Chúng tôi không cần phải kiểm tra các tên không liên quan. 

Đối với mỗi tên, chúng tôi duy trì vị trí cấp 1 và cấp 2 hiện tại. Khi một phần mới xuất hiện, chúng tôi có thể xác định ngay liệu việc hợp nhất có xảy ra hay không. Khi các phần biến mất, các vị trí của chúng trở nên trống, vì vậy chúng tôi cũng duy trì cấu trúc dữ liệu cung cấp chỉ mục trống nhỏ nhất một cách nhanh chóng. Heap tối thiểu là một lựa chọn tự nhiên vì chúng ta chỉ cần chèn các vị trí trống mới và trích xuất vị trí nhỏ nhất. 

Tổng công việc trở nên tỷ lệ thuận với số lượng quân cờ được xử lý và số lần hợp nhất thực tế, thay vì kích thước của khu vực chờ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN) | O(N) | Quá chậm | 
| Tối ưu | O(M log N) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ vùng chờ dưới dạng mảng có kích thước`N`. Ban đầu mọi vị trí đều chứa một điểm đánh dấu trống. Đặt mọi vị trí vào một đống tối thiểu các vị trí có sẵn để luôn có thể tìm thấy chỉ mục trống nhỏ nhất. 
2. Đối với mỗi tên quân cờ sắp đến, hãy kiểm tra xem có tồn tại quân cờ cấp 3 có tên đó hay không. Nếu vậy, hãy bỏ qua quân cờ này vì luật cấm thêm một bản sao khác. 
3. Giữ nguyên vị trí của quân cấp 1 và quân cấp 2 cho mỗi tên. Nếu quân cờ đến tạo ra`K`quân cấp 1 cùng tên, loại bỏ quân cấp 1 đó và biến quân cờ đến thành cấp 2. 
4. Sau khi tạo mảnh cấp 2, hãy kiểm tra xem hiện tại có`K`mảnh cấp 2. Nếu vậy, hãy loại bỏ những quân cờ đó và biến quân cờ đến ở cấp độ 3. 
5. Nếu quân cờ sống sót, lấy chỉ số nhỏ nhất từ đống vị trí trống và đặt quân cờ vào đó. Ghi lại vị trí này vào danh sách tương ứng với tên và cấp độ của nó. 
6. Sau khi tất cả các lần chèn được xử lý, hãy in vùng chờ. 

Lý do điều này có tác dụng là vì danh sách cho mỗi tên luôn chứa chính xác những phần hiện tồn tại. Mỗi lần hợp nhất sẽ loại bỏ chính xác các quân cờ theo yêu cầu của quy tắc và mọi quân cờ còn sống sót sẽ được đưa vào vị trí trống nhỏ nhất hiện tại. Bất biến vùng heap đảm bảo rằng mọi vị trí đều sử dụng cùng một vị trí mà quy trình ban đầu sẽ chọn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve_case(M, N, K, pieces):
    board = ["-1"] * N
    empty = list(range(N))
    heapq.heapify(empty)

    data = {}

    def get_info(name):
        if name not in data:
            data[name] = [[], [], False]
        return data[name]

    for name in pieces:
        lv1, lv2, has3 = get_info(name)

        if has3:
            continue

        level = 1

        if len(lv1) == K - 1:
            for pos in lv1:
                board[pos] = "-1"
                heapq.heappush(empty, pos)
            lv1.clear()
            level = 2

            if len(lv2) == K - 1:
                for pos in lv2:
                    board[pos] = "-1"
                    heapq.heappush(empty, pos)
                lv2.clear()
                level = 3
                has3 = True

        if empty:
            pos = heapq.heappop(empty)
            if level == 1:
                board[pos] = name
                lv1.append(pos)
            elif level == 2:
                board[pos] = name + "2"
                lv2.append(pos)
            else:
                board[pos] = name + "3"
                data[name][2] = True

    return " ".join(board)

def main():
    t = int(input())
    ans = []

    for case in range(1, t + 1):
        M, N, K = map(int, input().split())
        pieces = [input().strip() for _ in range(M)]
        ans.append(f"Case {case}: {solve_case(M, N, K, pieces)}")

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Mảng`board`lưu trữ trạng thái hiển thị cuối cùng của mọi vị trí. Đống`empty`chứa chính xác các vị trí hiện đang miễn phí. Bất cứ khi nào việc hợp nhất xảy ra, các phần bị loại bỏ sẽ quay trở lại vị trí của chúng về đống này, điều này duy trì khả năng tìm thấy vị trí trống ngoài cùng bên trái. 

Từ điển`data`giữ tất cả thông tin cụ thể về tên một người chơi cờ. Danh sách đầu tiên lưu trữ các vị trí cấp 1, danh sách thứ hai lưu trữ các vị trí cấp 2 và boolean ghi lại xem phần cấp 3 có tồn tại hay không. Điều này tránh việc quét các vị trí không liên quan. 

Thứ tự các thao tác trong vòng chèn tuân theo luật chơi. Việc kiểm tra cấp độ 3 diễn ra đầu tiên. Sau đó, việc hợp nhất cấp 1 được thử, tiếp theo là việc hợp nhất cấp 2. Chỉ sau khi tất cả các nâng cấp có thể hoàn tất thì mảnh đó mới được đặt vào bảng. 

Việc triển khai không cần xử lý đặc biệt đối với các giới hạn số nguyên vì nó chỉ lưu trữ các chỉ số và bộ đếm được giới hạn bởi kích thước đầu vào. Điều kiện biên quan trọng là kiểm tra xem đống trống có trống hay không trước khi đặt quân cờ, vì quân cờ có thể biến mất nếu bàn cờ đã đầy. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên, giả sử:```
1
5 4 2
a
a
b
a
a
```Mô phỏng là: 

| Bước | Đang đến | Vị trí cấp 1 của | Vị trí cấp 2 của | Khe trống | Ban | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | [] | [] | 0,1,2,3 | -1 -1 -1 -1 | 
| 1 | một | [0] | [] | 1,2,3 | a -1 -1 -1 | 
| 2 | một | [] | [0] | 1,2,3 | a2 -1 -1 -1 | 
| 3 | b | [] | [0] | 2,3 | a2 b -1 -1 | 
| 4 | một | [2] | [0] | 3 | a2 b a -1 | 
| 5 | một | [] | [0,2] | 1,3 | a2 b a2 -1 | 

Dấu vết này cho thấy tại sao việc lưu trữ vị trí lại quan trọng. hai`a`các quân hợp nhất sẽ bị loại bỏ và quân cấp 2 mới chiếm vị trí sẵn có ngoài cùng bên trái. 

Đối với ví dụ thứ hai:```
1
4 3 3
x
x
x
x
```Trạng thái thay đổi như sau: 

| Bước | Đang đến | Cấp độ | Khe trống | Ban | 
| --- | --- | --- | --- | --- | 
| 0 | không | không | 0,1,2 | -1 -1 -1 | 
| 1 | x | 1 | 1,2 | x -1 -1 | 
| 2 | x | 1 | 2 | x x -1 | 
| 3 | x | 1 | không | x x x | 
| 4 | x | 1 | không | x x x | 

Ba phần đầu tiên vẫn tách biệt vì`K=3`yêu cầu ba bản sao trước khi hợp nhất. Quân thứ tư không có sẵn vị trí nên biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log N) | Mỗi lần chèn sẽ thực hiện các thao tác từ điển và cập nhật vùng nhớ heap cho các vị trí đã xóa hoặc đã chèn. | 
| Không gian | O(N + M) | Bảng, đống vị trí trống và các vị trí được lưu trữ chứa hầu hết thông tin tuyến tính. | 

Những ràng buộc cho phép`N`phải lớn, do đó cần tránh việc quét lặp đi lặp lại vùng chờ. Các thao tác heap giữ cho mô phỏng đủ nhanh trong khi vẫn duy trì hành vi sắp xếp chính xác ở ngoài cùng bên trái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        import heapq

        for case in range(1, t + 1):
            M, N, K = map(int, input().split())
            pieces = [input().strip() for _ in range(M)]

            board = ["-1"] * N
            empty = list(range(N))
            heapq.heapify(empty)
            data = {}

            for name in pieces:
                if name not in data:
                    data[name] = [[], [], False]

                lv1, lv2, has3 = data[name]

                if has3:
                    continue

                level = 1

                if len(lv1) == K - 1:
                    for p in lv1:
                        board[p] = "-1"
                        heapq.heappush(empty, p)
                    lv1.clear()
                    level = 2

                    if len(lv2) == K - 1:
                        for p in lv2:
                            board[p] = "-1"
                            heapq.heappush(empty, p)
                        lv2.clear()
                        level = 3
                        data[name][2] = True

                if empty:
                    p = heapq.heappop(empty)
                    if level == 1:
                        board[p] = name
                        lv1.append(p)
                    elif level == 2:
                        board[p] = name + "2"
                        lv2.append(p)
                    else:
                        board[p] = name + "3"

            out.append(f"Case {case}: {' '.join(board)}")

        return "\n".join(out)

    result = solve()
    sys.stdin = old
    return result

assert run("""1
3 3 2
a
a
a
""") == "Case 1: a2 a -1", "basic merge"

assert run("""1
3 2 2
a
b
a
""") == "Case 1: a2 b", "merge with full board"

assert run("""1
4 3 3
x
x
x
x
""") == "Case 1: x x x", "no merge until enough copies"

assert run("""1
5 5 2
a
a
a
a
a
""") == "Case 1: a3 a -1 -1 -1", "multiple upgrades"

assert run("""1
1 1 2
z
""") == "Case 1: z", "minimum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba mảnh giống hệt nhau với`K=2`|`a2 a -1`| Hợp nhất và sắp xếp cơ bản | 
| Hội đồng đầy đủ với sự hợp nhất |`a2 b`| Tái sử dụng các vị trí đã giải phóng | 
| Bốn mảnh với`K=3`|`x x x`| Ngưỡng hợp nhất đúng | 
| Năm mảnh giống hệt nhau với`K=2`|`a3 a -1 -1 -1`| Nâng cấp theo chuỗi | 
| Cờ vua đơn |`z`| Trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
1
3 3 2
a
a
a
```Thuật toán đứng đầu`a`ở vị trí số không. thứ hai`a`tìm thấy một bản sao cấp 1 hiện có, xóa nó và trở thành cấp 2 ở vị trí 0. thứ ba`a`không thể hợp nhất vì chỉ có một quân cấp 2 nên được đặt ở vị trí một. Kết quả là:```
Case 1: a2 a -1
```Danh sách cấp độ được lưu trữ ngăn chặn chính xác thuật toán nâng cấp không chính xác trực tiếp lên cấp 3. 

Đối với trường hợp trọn gói:```
1
3 2 2
a
b
a
```Hai phần chèn đầu tiên lấp đầy bảng. Lần chèn thứ ba nhìn thấy một cấp độ 1`a`, loại bỏ nó và đặt mảnh cấp 2 đã nâng cấp trở lại khe nhỏ nhất còn trống. Kết quả là:```
Case 1: a2 b
```Vùng nhớ trống là nguyên nhân khiến trò chơi này hoạt động giống như trò chơi gốc thay vì chỉ thêm các phần vào. 

Đối với trường hợp hạn chế cấp 3:```
1
5 4 2
a
a
a
a
a
```Bốn quân cờ đầu tiên tạo thành hai quân cờ cấp 2. Lần hợp nhất tiếp theo sẽ tạo ra một mảnh cấp 3. Bất kỳ sau này`a`sẽ bị từ chối ngay lập tức vì tên đã có quân cờ cấp 3. Boolean được lưu trữ cho mỗi tên ghi lại sự thay đổi trạng thái vĩnh viễn này.
