---
title: "CF 102709G - Chăn nhân vật"
description: "Bài toán yêu cầu chúng ta xây dựng một bức tranh nhân vật lớn từ những ô vuông nhỏ hơn. Chúng ta được cung cấp một bộ sưu tập các ô cơ sở, trong đó mỗi ô là một lưới ký tự S x S. Tấm chăn cuối cùng được sắp xếp theo hình chữ nhật W x H với các vị trí xếp gạch."
date: "2026-08-01T22:00:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102709
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 2"
rating: 0
weight: 102709
solve_time_s: 122
verified: true
draft: false
---

[CF 102709G - Chăn nhân vật](https://codeforces.com/problemset/problem/102709/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xây dựng một bức tranh nhân vật lớn từ những ô vuông nhỏ hơn. Chúng ta được cung cấp một bộ sưu tập các ô cơ sở, trong đó mỗi ô là một lưới ký tự S x S. Tấm chăn cuối cùng được sắp xếp theo hình chữ nhật W x H với các vị trí xếp gạch. Mỗi vị trí cho chúng ta biết nên sử dụng ô cơ sở nào và áp dụng phép biến đổi nào trước khi đặt nó vào hình ảnh cuối cùng. 

Các phép biến đổi được giới hạn trong sáu trường hợp: giữ nguyên ô, xoay ô 90, 180 hoặc 270 độ theo chiều kim đồng hồ, lật từ trái sang phải hoặc lật từ trên xuống dưới. Đầu ra là lưới ký tự thu được sau khi mở rộng mọi vị trí ô xếp thành khối S by S được chuyển đổi của nó. 

Các ràng buộc được thiết kế để thực hiện mô phỏng. Số lượng ô ban đầu và chiều dài cạnh của mỗi ô nhiều nhất là 15, vì vậy việc lưu trữ và chuyển đổi tất cả các ô sẽ rẻ. Chăn có thể chứa tối đa 100 x 100 vị trí ô và mỗi ô đóng góp tối đa 15 x 15 ký tự. Đầu ra cuối cùng có thể chứa 2.250.000 ký tự, do đó, mọi cách tiếp cận tránh được công việc không cần thiết đều được ưu tiên. Một phương pháp liên tục tìm kiếm, sao chép hoặc biến đổi các cấu trúc lớn cho mỗi ô đầu ra sẽ có nguy cơ trở nên chậm hơn mức cần thiết, trong khi việc xử lý trực tiếp từng ký tự cuối cùng lại đủ hiệu quả. 

Các lỗi triển khai chính đến từ việc xử lý các phép biến đổi không chính xác. Các thao tác xoay đặc biệt dễ dàng đảo ngược vì cả hàng và cột nguồn đều thay đổi. Một lỗi phổ biến khác là nhầm lẫn giữa lật ngang và lật dọc. Ví dụ, một viên ngói```
ab
cd
```với phép biến đổi 4 trở thành```
ba
dc
```vì các cột bị đảo ngược. Một chương trình coi việc này như việc hoán đổi hàng sẽ xuất ra không chính xác```
cd
ab
```Đối với một ô đơn, đầu vào```
1 2
ab
cd
1 1
0:1
```nên sản xuất```
ca
db
```bởi vì gạch được xoay theo chiều kim đồng hồ. Việc triển khai sử dụng công thức xoay ngược chiều kim đồng hồ sẽ tạo ra sự sắp xếp sai. 

Một trường hợp cạnh khác là chăn chỉ có một vị trí ô. đầu vào```
1 1
x
1 1
0:0
```phải xuất ra```
x
```Giải pháp giả định nhiều ô và chèn dấu phân cách hoặc xử lý sai độ dài hàng có thể không thành công ở đây. 

Trường hợp cạnh thứ ba là khi các phép biến đổi khác nhau của cùng một ô xuất hiện cạnh nhau. Chương trình không thể thay đổi ô gốc được lưu trữ sau khi áp dụng một phép chuyển đổi, vì việc sử dụng cùng một ô sau này cần có phiên bản nguyên vẹn. Ví dụ: sử dụng một ô hai lần với các phép biến đổi 1 và 2 yêu cầu cả hai thao tác phải bắt đầu từ lưới ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xử lý mọi vị trí quilt, xác định vị trí ô nguồn của nó, áp dụng phép biến đổi được yêu cầu và in khối S by S kết quả. Điều này đã đúng vì mỗi ký tự đầu ra thuộc về chính xác một vị trí ô và mọi vị trí ô có thể được xử lý độc lập. 

Vấn đề với việc triển khai đơn giản chỉ xuất hiện nếu các phép biến đổi được tính toán lại ở cấp độ ký tự. Đối với mỗi vị trí ô W nhân H, chúng ta có thể chạm vào S ký tự bình phương. Trường hợp xấu nhất thực hiện các phép toán W * H * S * S. Với W và H bằng 100 và S bằng 15, đây là phép toán 2.250.000 ký tự, một con số nhỏ. Mối nguy hiểm thực sự không phải là sự phức tạp tiệm cận mà là việc liên tục tạo ra các lưới trung gian không cần thiết. 

Quan sát quan trọng là tập hợp các phép biến đổi rất nhỏ và các ô ban đầu được sử dụng lại nhiều lần. Vì chỉ có sáu phép biến đổi có thể thực hiện được nên mỗi ô ban đầu có thể được mở rộng thành tất cả sáu phiên bản đã biến đổi một lần. Sau quá trình tiền xử lý đó, mọi vị trí chăn sẽ trở thành một tra cứu đơn giản, sau đó sao chép S hàng vào câu trả lời. 

Brute-force hoạt động vì tổng số ký tự đầu ra bị giới hạn. Nhận thấy rằng mỗi phép chuyển đổi ô có thể được lưu vào bộ nhớ đệm cho phép chúng tôi loại bỏ công việc lặp lại và giữ cho việc triển khai trở nên đơn giản. Việc xây dựng cuối cùng chỉ là lắp ráp các khối đã được chuẩn bị sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(W * H * S * S) | O(S * S) | Được chấp nhận nhưng lặp lại các phép biến đổi | 
| Tối ưu | O(N * 6 * S * S + W * H * S * S) | O(N * 6 * S * S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ô gốc và lưu trữ chúng mà không sửa đổi. Hướng ban đầu phải vẫn có sẵn vì nhiều vị trí chăn có thể tham chiếu cùng một ô với các phép biến đổi khác nhau. 
2. Với mỗi ô, hãy tạo sáu phiên bản biến đổi có thể có. Lưu trữ chúng trong một bảng được lập chỉ mục theo số ô và loại chuyển đổi. Điều này chuyển đổi các yêu cầu chuyển đổi trong tương lai thành tra cứu theo thời gian liên tục. 
3. Đọc mô tả chăn bông theo từng hàng. Mỗi mục chứa một chỉ mục khối ảnh và một số biến đổi, do đó nó xác định trực tiếp một trong các khối được tính toán trước. 
4. Đối với mỗi hàng chăn, nối các hàng ô chuyển đổi tương ứng theo chiều ngang. Vị trí quilt đóng góp S ký tự cho mỗi S hàng đầu ra mà nó chiếm giữ. 
5. In các hàng đã tạo theo thứ tự. Các hàng đã hoàn tất vì mỗi vị trí ô xếp đều đóng góp chính xác phần hình chữ nhật của riêng nó. 

Tại sao nó hoạt động: 

Điều bất biến là mọi ô biến đổi được lưu trữ chính xác là kết quả của việc áp dụng phép biến đổi của nó vào ô ban đầu. Vì mỗi vị trí quilt chỉ yêu cầu một ô và một phép biến đổi nên việc truy xuất phiên bản đã lưu trong bộ nhớ đệm sẽ cung cấp khối chính xác thuộc về khối đó. Quá trình xây dựng duy trì thứ tự từ trái sang phải và từ trên xuống dưới của các vị trí ô, do đó mỗi ô đầu ra đều nhận được ký tự chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rotate90(tile):
    n = len(tile)
    return [''.join(tile[n - 1 - r][c] for r in range(n)) for c in range(n)]

def flip_vertical(tile):
    return [row[::-1] for row in tile]

def flip_horizontal(tile):
    return tile[::-1]

def solve():
    n, s = map(int, input().split())

    tiles = []
    for _ in range(n):
        tile = [input().rstrip('\n') for _ in range(s)]
        tiles.append(tile)

    transformed = []
    for tile in tiles:
        versions = [None] * 6
        versions[0] = tile
        versions[1] = rotate90(versions[0])
        versions[2] = rotate90(versions[1])
        versions[3] = rotate90(versions[2])
        versions[4] = flip_vertical(versions[0])
        versions[5] = flip_horizontal(versions[0])
        transformed.append(versions)

    w, h = map(int, input().split())

    answer = ['' for _ in range(h * s)]

    for quilt_row in range(h):
        specs = input().split()

        for tile_row in range(s):
            line = []
            for spec in specs:
                idx, t = map(int, spec.split(':'))
                line.append(transformed[idx][t][tile_row])
            answer[quilt_row * s + tile_row] = ''.join(line)

    sys.stdout.write('\n'.join(answer))

if __name__ == "__main__":
    solve()
```Phần tiền xử lý tạo ra tất cả sáu hướng cho mỗi ô. Các phép quay được xâu chuỗi sao cho việc áp dụng thao tác 90 độ ba lần sẽ tạo ra các phép quay còn lại một cách tự nhiên. Hai lần lật sử dụng trực tiếp ô gốc vì các lần lật không phụ thuộc vào các lần quay trước đó. 

Giai đoạn xây dựng tránh thay đổi bất kỳ ô nào được lưu trữ. Mỗi thông số kỹ thuật được chia thành chỉ số khối ảnh và giá trị biến đổi, sau đó hàng yêu cầu của khối ảnh đã được chuyển đổi sẽ được thêm vào. Tính toán chỉ số hàng`quilt_row * s + tile_row`ánh xạ một hàng ô bên trong chăn tới đúng hàng trong lưới ký tự cuối cùng. 

Không có trường hợp ranh giới nguy hiểm nào trong việc lập chỉ mục vì mọi phép biến đổi đều giữ kích thước ô bằng S. Ở đây, số nguyên Python không phải là vấn đề đáng lo ngại vì chương trình chỉ lưu trữ các chỉ mục và số lượng ký tự. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào nhỏ:```
1 2
ab
cd
2 1
0:0 0:1
0:2 0:3
```Quá trình tiền xử lý tạo ra: 

| Ngói | Chuyển đổi | Hàng được lưu trữ | 
| --- | --- | --- | 
| 0 | 0 | ab/cd | 
| 0 | 1 | ca/db | 
| 0 | 2 | dc/ba | 
| 0 | 3 | bd/ac | 

Cấu trúc của chăn là: 

| Hàng chăn | Thông số kỹ thuật ngói | Hàng đầu ra được sản xuất | 
| --- | --- | --- | 
| 0 | 0:0, 0:1 | abca / cddb | 
| 1 | 0:2, 0:3 | dcbd / baac | 

Kết quả chứng minh rằng mọi vị trí ô đều độc lập và có thể sử dụng hướng khác nhau của cùng một ô ban đầu. 

Một ví dụ thứ hai:```
1 1
*
1 1
0:5
```Dấu vết trạng thái là: 

| Bước | Chỉ số gạch | Chuyển đổi | Khối đã chọn | 
| --- | --- | --- | --- | 
| Đọc gạch | 0 | bản gốc | * | 
| Tính toán trước | 0 | lật ngang | * | 
| Đặt gạch | 0 | 5 | * | 

Đầu ra là:```
*
```Điều này xác nhận rằng ô một ký tự vẫn hợp lệ trong mọi phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * 6 * S * S + W * H * S * S) | Mỗi phép biến đổi được chuẩn bị một lần, sau đó mỗi ô chăn sẽ sao chép một ký tự | 
| Không gian | O(N * 6 * S * S) | Sáu phiên bản của mỗi ô được lưu trữ | 

Sản lượng lớn nhất có thể chi phối chi phí xây dựng. Vì bản thân hình ảnh cuối cùng chỉ chứa vài triệu ký tự nên thuật toán vẫn nằm trong giới hạn đồng thời tránh được công việc chuyển đổi lặp đi lặp lại. 

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

assert run("""1 1
x
1 1
0:0
""") == "x", "single character tile"

assert run("""1 2
ab
cd
1 1
0:1
""") == "ca\ndb", "rotation"

assert run("""1 2
ab
cd
2 1
0:4
""") == "ba\ndc", "vertical flip"

assert run("""1 2
ab
cd
1 1
0:5
""") == "cd\nab", "horizontal flip"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô một ký tự | x | Xử lý kích thước tối thiểu | 
| Xoay hai viên gạch | ca/db | Công thức xoay | 
| Lật dọc hai ô theo chiều dọc | ba/dc | Đảo ngược cột | 
| Lật ngang hai viên gạch | cd / ab | Đảo ngược hàng | 

## Vỏ cạnh 

Đối với một vị trí ô xếp, thuật toán vẫn tạo cùng một bộ nhớ đệm được chuyển đổi và thực hiện một lần tra cứu thông thường. đầu vào```
1 1
x
1 1
0:0
```chọn phiên bản được lưu trong bộ nhớ cache duy nhất và in một ký tự. Không có trường hợp đặc biệt nào vì công trình chung đã xử lý được chiếc chăn bông nhỏ nhất có thể. 

Đối với các phép biến đổi của cùng một ô, ô ban đầu không bao giờ bị ghi đè. Nếu ngói```
ab
cd
```được yêu cầu với các phép biến đổi 1 và 2, bộ đệm chứa cả hai```
ca
db
```Và```
dc
ba
```cùng một lúc. Điều này ngăn các yêu cầu sau này phụ thuộc vào các hoạt động đầu ra trước đó. 

Đối với các ô được biến đổi khác nhau liền kề, thuật toán ghi riêng từng hàng ô và ghép các mảnh theo thứ tự quilt. Ô bên trái không thể ảnh hưởng đến ô bên phải vì mỗi vị trí đóng góp chính xác S ký tự cho các hàng đầu ra hiện tại.
