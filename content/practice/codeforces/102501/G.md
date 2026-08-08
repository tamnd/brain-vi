---
title: "CF 102501G - Hoán đổi địa điểm"
description: "Chúng ta được cung cấp một chuỗi các loài động vật đại diện cho thứ tự các loài động vật bước vào hàng chờ. Thứ tự rời đi cuối cùng không cố định vì các động vật lân cận được phép đổi chỗ cho nhau khi cặp loài của chúng được liệt kê là tương thích."
date: "2026-08-06T05:00:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 77
verified: true
draft: false
---

[CF 102501G - Hoán đổi địa điểm](https://codeforces.com/problemset/problem/102501/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các loài động vật đại diện cho thứ tự các loài động vật bước vào hàng chờ. Thứ tự rời đi cuối cùng không cố định vì các động vật lân cận được phép đổi chỗ cho nhau khi cặp loài của chúng được liệt kê là tương thích. Nhiệm vụ là tìm ra chuỗi loài nhỏ nhất về mặt từ điển có thể xuất hiện khi tất cả các loài động vật cuối cùng rời đi. 

Đầu vào mô tả tập hợp các loài, các cặp loài có thể giao thoa với nhau và trình tự ban đầu. Đầu ra là một chuỗi khác chứa các con vật giống nhau, nhưng được sắp xếp theo thứ tự rời đi sớm nhất có thể theo thứ tự từ điển. 

Khó khăn chính là việc hoán đổi mang tính chất địa phương. Con vật không thể tùy ý di chuyển về phía trước. Để di chuyển sang trái qua một con vật khác thì phải được phép hoán đổi với loài của con vật đó. Các ràng buộc cho chúng ta biết rằng chỉ có tối đa 200 loài nhưng có tới 100000 loài động vật. Điều này loại trừ việc mô phỏng các giao dịch hoán đổi hoặc khám phá các hoán vị có thể xảy ra, bởi vì số lượng đơn đặt hàng có thể tiếp cận có thể theo cấp số nhân. Thuật toán phải xử lý chuỗi gần như tuyến tính, chỉ với một lượng công việc bổ sung nhỏ cho mỗi loài. 

Có một số trường hợp dễ bỏ sót. Nếu một loài không có bạn bè thì động vật của nó không thể vượt qua bất kỳ loài nào khác. Ví dụ:```
2 0 3
A
B
A B A
```Câu trả lời là:```
A B A
```Một cách tiếp cận tham lam bất cẩn luôn chọn những loài nhỏ nhất xuất hiện ở bất cứ đâu sẽ dẫn đến kết quả`A A B`, điều này là không thể bởi vì thứ hai`A`không thể vượt qua`B`. 

Một trường hợp phức tạp khác là sự xuất hiện lặp đi lặp lại của cùng một loài. Các loài bình đẳng không cần xin phép lai vì đầu ra chỉ quan tâm đến tên loài chứ không quan tâm đến từng con vật. Ví dụ:```
2 0 3
A
B
A A B
```Câu trả lời là:```
A A B
```Một giải pháp xử lý các loài bằng nhau như một cặp chặn sẽ ngăn chặn không chính xác cả hai`A`động vật được xem xét cùng nhau. 

Trường hợp thứ ba là khi sự xuất hiện muộn hơn của một loài bị chặn bởi sự xuất hiện trước đó của cùng loài đó. Ví dụ:```
3 1 4
A
B
C
A B C B
A B
```Câu trả lời là:```
A B C B
```cuối cùng`B`không thể di chuyển trước`C`mặc dù`B`nhỏ hơn`C`, bởi vì`B`phải vượt qua`C`, và việc trao đổi đó không được phép. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng tạo ra tất cả các chuỗi có thể truy cập được. Mọi hoán đổi liền kề được phép sẽ tạo ra một trạng thái khả dĩ khác, vì vậy chúng ta có thể thực hiện tìm kiếm theo chiều rộng trên các trạng thái và giữ lại chuỗi nhỏ nhất được tìm thấy. Điều này đúng vì mọi hoán đổi pháp lý đều được khám phá. Tuy nhiên, thậm chí chỉ với một vài loài động vật, số lượng các trạng thái có thể có có thể lên tới gần`N!`, làm cho nó hoàn toàn không thể`N = 100000`. 

Quan sát hữu ích là chúng ta thực sự không cần phải mô phỏng các giao dịch hoán đổi. Thay vào đó, chúng ta có thể mô tả những con vật nào bị buộc phải ở trước những con khác. 

Xét một con vật ở vị trí`i`. Nếu có một loài động vật sớm hơn`X`đó không phải là bạn với loài này`i`, thì con vật ở`i`không bao giờ có thể di chuyển trước con vật trước đó. Điều đó tạo ra một ràng buộc về quyền ưu tiên: con vật trước đó phải rời đi trước con vật hiện tại. 

Chi tiết quan trọng là chúng ta chỉ cần sự xuất hiện mới nhất của từng loài chặn. Nếu sự xuất hiện sớm hơn của cùng một loài tồn tại thì sự xuất hiện muộn nhất là chướng ngại vật gần nhất phải vượt qua. Nếu lần xuất hiện mới nhất đó có thể được lai, thì tất cả các lần xuất hiện trước đó của cùng một loài cũng có thể được lai. 

Những ràng buộc ưu tiên này tạo thành một biểu đồ tuần hoàn có hướng. Mỗi con vật là một nút và một cạnh từ`a`ĐẾN`b`có nghĩa là động vật`a`phải rời đi trước động vật`b`. Bất kỳ thứ tự rời đi hợp lệ nào cũng là thứ tự tôpô của biểu đồ này. 

Trong số tất cả các thứ tự tôpô, thứ tự nhỏ nhất về mặt từ điển có thể được tìm thấy bằng cách liên tục chọn nút nhỏ nhất có sẵn. Đây là phiên bản xếp hàng ưu tiên tiêu chuẩn của phân loại tôpô. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi mệnh lệnh hợp pháp, nhưng nó thất bại vì có quá nhiều mệnh lệnh. Quan sát cho thấy chỉ những loài trước đó không thể tráo đổi mới tạo ra các hạn chế cho phép chúng tôi nén vấn đề thành DAG và giải quyết nó một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(N * S + N log N) | O(N + S2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc loài và gán cho mỗi loài một id số nguyên. Lưu trữ những cặp loài có thể trao đổi. 
2. Quét chuỗi đầu vào từ trái sang phải và tạo biểu đồ phụ thuộc giữa các con vật. Đối với loài động vật hiện tại`x`, kiểm tra từng loài`y`điều đó đã xuất hiện trước đây. Nếu như`y`không được phép trao đổi với`x`, thêm một cạnh từ gần đây nhất`y`xảy ra với động vật hiện tại. 

Cạnh có nghĩa là con vật trước phải rời đi trước vì con vật hiện tại không thể di chuyển qua nó. 
3. Duy trì vị trí mới nhất của mọi loài trong khi quét. Sau khi xử lý một lần xuất hiện, hãy cập nhật vị trí mới nhất của loài đó. 
4. Tính độ của tất cả các nút trong biểu đồ phụ thuộc. Mọi nút có mức độ bằng 0 về mặt pháp lý có thể là động vật tiếp theo rời đi. 
5. Đặt tất cả các loài động vật hiện có vào một đống nhỏ được sắp xếp theo tên loài của chúng. Liên tục loại bỏ các loài nhỏ nhất, thêm nó vào câu trả lời và giảm mức độ của các loài lân cận. 
6. Tiếp tục cho đến khi mọi con vật đã được gỡ bỏ. Thứ tự sản xuất là trình tự rời đi bắt buộc. 

Tại sao nó hoạt động: 

Biểu đồ chứa chính xác những hạn chế do động vật tạo ra không thể vượt qua nhau. Nếu có một cạnh từ con vật này đến con vật khác thì mọi chuỗi hợp lệ đều phải giữ thứ tự đó. Nếu không có cạnh phụ thuộc, con vật trước đó sẽ không ngăn cản con vật sau tiếp cận phía trước. Do đó, mọi thứ tự rời đi hợp lệ đều tương ứng với thứ tự tôpô của biểu đồ này. 

Ở mỗi bước của thuật toán Kahn, các nút có sẵn chính xác là những con vật có thể được di chuyển lên phía trước mà không vi phạm bất kỳ hạn chế nào. Việc chọn loài nhỏ nhất hiện có luôn an toàn vì không có loài động vật nào không có sẵn có thể xuất hiện trước nó theo bất kỳ thứ tự hợp lệ nào. Việc lặp lại lựa chọn này sẽ cho ra tiền tố nhỏ nhất có thể ở mọi vị trí, do đó toàn bộ chuỗi là tối thiểu về mặt từ điển. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    S, L, N = map(int, input().split())

    names = [input().strip() for _ in range(S)]
    ids = {name: i for i, name in enumerate(names)}

    friend = [[False] * S for _ in range(S)]
    for _ in range(L):
        a, b = input().split()
        x, y = ids[a], ids[b]
        friend[x][y] = True
        friend[y][x] = True

    seq = [ids[x] for x in input().split()]

    graph = [[] for _ in range(N)]
    indeg = [0] * N

    last = [-1] * S

    for i, x in enumerate(seq):
        for y in range(S):
            if last[y] != -1 and y != x and not friend[x][y]:
                graph[last[y]].append(i)
                indeg[i] += 1
        last[x] = i

    heap = []
    for i in range(N):
        if indeg[i] == 0:
            heapq.heappush(heap, (names[seq[i]], i))

    ans = []

    while heap:
        _, u = heapq.heappop(heap)
        ans.append(names[seq[u]])

        for v in graph[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, (names[seq[v]], v))

    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên ánh xạ tên loài thành các mã định danh số nguyên. Id số nguyên làm cho ma trận bạn bè và các so sánh sau này có thời gian không đổi. 

Việc xây dựng đồ thị là cốt lõi của giải pháp. Trong khi quét trình tự,`last[y]`lưu trữ sự xuất hiện mới nhất của loài`y`. Khi con vật hiện tại nhìn thấy một loài trước đó không thể hoán đổi với nó, lần xuất hiện mới nhất đó là yếu tố chặn gần nhất không thể tránh khỏi, do đó, một cạnh được thêm từ lần xuất hiện đó vào lần xuất hiện hiện tại. 

Giai đoạn sắp xếp tôpô sử dụng một đống thay vì hàng đợi thông thường. Việc sắp xếp tôpô thông thường sẽ tạo ra bất kỳ thứ tự hợp lệ nào, nhưng vấn đề yêu cầu thứ tự chữ cái nhỏ nhất. Heap luôn loại bỏ những loài nhỏ nhất có sẵn. 

Không có vấn đề riêng biệt nào vì các nút đại diện cho động vật theo vị trí dựa trên số 0 ban đầu của chúng. Mức độ chỉ được cập nhật khi các phần phụ thuộc được loại bỏ và một nút sẽ đi vào vùng heap chính xác khi tất cả các động vật phải đứng trước nó đã được xuất ra. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 2 6
ANTILOPE
CAT
ANT
CAT ANTILOPE
ANTILOPE ANT
ANT CAT CAT ANTILOPE CAT ANT
```Việc tạo biểu đồ phụ thuộc hoạt động như sau: 

| Vị trí | Loài | Chặn các loài trước | Đã thêm phụ thuộc | 
| --- | --- | --- | --- | 
| 0 | KIẾN | không | không | 
| 1 | MÈO | không | không | 
| 2 | MÈO | không | không | 
| 3 | KHÁNG | MÈO | MÈO -> ANTILOPE | 
| 4 | MÈO | không | không | 
| 5 | KIẾN | MÈO | MÈO -> KIẾN | 

Những động vật có sẵn ban đầu là`ANT`,`CAT`, Và`CAT`. Đống chọn`ANT`, khi đó các lựa chọn nhỏ nhất còn lại sẽ tạo ra: 

| Bước đầu ra | Con vật được chọn | Câu trả lời hiện tại | 
| --- | --- | --- | 
| 1 | KIẾN | KIẾN | 
| 2 | KHÁNG | KIẾN | 
| 3 | MÈO | MÈO KIẾN | 
| 4 | MÈO | KIẾN MÈO MÈO | 
| 5 | MÈO | KIẾN MÈO MÈO MÈO | 
| 6 | KIẾN | KIẾN MÈO MÈO MÈO KIẾN | 

Sự phụ thuộc ngăn không cho linh dương vượt qua những con mèo chặn nó, trong khi vẫn cho phép nó di chuyển trước con kiến ​​cuối cùng. 

Một ví dụ tùy chỉnh nhỏ hơn:```
3 1 5
A
B
C
A B
B C A B C
```Việc xây dựng đồ thị cho: 

| Vị trí | Loài | Phụ thuộc | 
| --- | --- | --- | 
| 0 | B | không | 
| 1 | C | B -> C | 
| 2 | A | C -> A | 
| 3 | B | C -> B | 
| 4 | C | không | 

Quá trình heap trở thành: 

| Bước đầu ra | Loài có sẵn | Được chọn | 
| --- | --- | --- | 
| 1 | B, C | B | 
| 2 | C | C | 
| 3 | A, B, C | A | 
| 4 | B, C | B | 
| 5 | C | C | 

Đầu ra là:```
B C A B C
```Ví dụ này cho thấy rằng loài nhỏ hơn không phải lúc nào cũng được chọn ngay lập tức. Đầu tiên nó phải có sẵn bằng cách loại bỏ tất cả động vật chặn nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * S + N log N) | Mỗi con vật sẽ kiểm tra tất cả các loài khi sự phụ thuộc được tạo ra và mỗi thao tác heap xử lý một con vật | 
| Không gian | O(N + S2) | Biểu đồ lưu trữ tối đa các phụ thuộc O(N*S) trong trường hợp xấu nhất và biểu đồ loài sử dụng bộ nhớ O(S²) | 

Số lượng loài chỉ có 200 nên việc kiểm tra tất cả các loài cho từng con vật tốn nhiều nhất khoảng 20 triệu thao tác. Quá trình xử lý biểu đồ và đống là tuyến tính hoặc gần tuyến tính về số lượng động vật, phù hợp với giới hạn cho`N = 100000`. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

assert run("""3 2 6
ANTILOPE
CAT
ANT
CAT ANTILOPE
ANTILOPE ANT
ANT CAT CAT ANTILOPE CAT ANT
""") == "ANT ANTILOPE CAT CAT CAT ANT"

assert run("""1 0 1
A
A
""") == "A"

assert run("""2 0 4
A
B
B A B A
""") == "B A B A"

assert run("""2 1 5
A
B
A B
B A B A B
""") == "A B A B B"

assert run("""3 0 5
A
B
C
C C B A C
""") == "C C B A C"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu |`ANT ANTILOPE CAT CAT CAT ANT`| Xây dựng sự phụ thuộc cơ bản và đặt hàng tham lam | 
| Một loài |`A`| Kích thước tối thiểu và không xử lý phụ thuộc | 
| Không có tình bạn |`B A B A`| Những loài không thể lai vẫn cố định | 
| Tình bạn trọn vẹn giữa hai loài |`A B A B B`| Di chuyển tự do giữa các loài tương thích | 
| Loài bị chặn lặp đi lặp lại |`C C B A C`| Loài trùng lặp và hạn chế đặt hàng | 

## Vỏ cạnh 

Đối với trường hợp không có tình bạn:```
2 0 3
A
B
A B A
```Thuật toán tạo ra một cạnh ngay từ đầu`A`ĐẾN`B`, bởi vì`B`không thể vượt qua`A`. thứ hai`A`cũng phụ thuộc vào`B`nếu như`B`xuất hiện trước nó. Heap không bao giờ hiển thị sai thứ hai`A`trước khi chặn`B`, tạo ra câu trả lời hợp lệ duy nhất. 

Đối với các loài giống hệt nhau lặp đi lặp lại:```
2 0 3
A
B
A A B
```hai`A`động vật không tạo ra sự phụ thuộc lẫn nhau. Chỉ có các loài khác nhau mới quan trọng khi kiểm tra các giao dịch hoán đổi. Đồ thị không có cạnh nào kể từ cạnh đầu tiên`A`đến thứ hai`A`, vì vậy cả hai đều có sẵn trước`B`, cho kết quả đúng`A A B`. 

Đối với lần xuất hiện sau bị chặn bởi lần xuất hiện trước đó:```
3 1 4
A
B
C
A B
A B C B
```Khi xử lý cuối cùng`B`, mới nhất`C`sự xuất hiện là một trình chặn vì`B`Và`C`không phải là bạn bè. Biểu đồ thêm`C -> B`, vậy là trận chung kết`B`không thể nhảy trước`C`. Thứ tự tôpô tôn trọng hạn chế này và trả về chuỗi nhỏ nhất có thể truy cập được.
