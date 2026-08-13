---
title: "CF 102361K - MUV LUV KHÔNG GIỚI HẠN"
description: "Chúng ta có một cây có gốc với đỉnh 1 là gốc. Một bước di chuyển bao gồm việc xóa bất kỳ tập hợp các lá hiện tại nào khác trống, trong đó một lá là một đỉnh không còn con nào. Được phép loại bỏ nhiều lá cùng một lúc và cha mẹ của chúng có thể trở thành lá cho lần di chuyển tiếp theo."
date: "2026-08-13T00:18:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "K"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 150
verified: true
draft: false
---

[CF 102361K - MUV LUV UNLIMITED](https://codeforces.com/problemset/problem/102361/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với đỉnh 1 là gốc. Một bước di chuyển bao gồm việc xóa bất kỳ tập hợp các lá hiện tại nào khác trống, trong đó một lá là một đỉnh không còn con nào. Được phép loại bỏ nhiều lá cùng một lúc và cha mẹ của chúng có thể trở thành lá cho lần di chuyển tiếp theo. Những người chơi luân phiên nhau và người chơi loại bỏ đỉnh cuối cùng còn lại sẽ thắng vì người chơi kia khi đó không có nước đi hợp pháp. Nhiệm vụ là xác định xem vị trí ban đầu có giành chiến thắng cho người chơi đầu tiên hay không,`Takeru`, hoặc chiến thắng cho người chơi thứ hai,`Meiya`. Vấn đề chính thức sử dụng chính xác công thức cây gốc này và cho phép đạt tổng của tất cả các kích thước cây trong các trường hợp thử nghiệm (10^6). 

Mảng cha chứa cha của mọi đỉnh từ 2 đến (n). Chúng ta chỉ cần quan hệ cha và số con của mỗi đỉnh. Ràng buộc (n\le 10^6), cùng với giới hạn tương tự về tổng trên tất cả các trường hợp thử nghiệm, sẽ loại trừ mọi tìm kiếm trò chơi trong không gian trạng thái. Về nguyên tắc, thậm chí (O(n\log n)) có thể được chấp nhận, nhưng cấu trúc của trò chơi cho phép chúng ta giảm giải pháp về thời gian tuyến tính và bộ nhớ tuyến tính. 

Có hai trường hợp cạnh rất dễ xử lý sai. Đầu tiên, một đỉnh có cha mẹ có nhiều con có ý nghĩa quan trọng hơn nhiều so với một lá thông thường ở cuối chuỗi. Ví dụ,```
1
4
1 1 2
```có gốc 1 với con 2 và 3, đỉnh 2 có con 4. Đỉnh 3 là lá mà cha mẹ có hai con. Câu trả lời đúng là`Takeru`. Một giải pháp bất cẩn chỉ tính đến độ ngang bằng của mỗi chiếc lá có thể bỏ lỡ vị trí thắng lợi về mặt cấu trúc này. Lý do là Takeru có thể loại bỏ chiếc lá đó cùng với bất kỳ chiếc lá nào tạo thành phản ứng chiến thắng sau khi chiếc lá đó bị loại bỏ. 

Trường hợp cạnh thứ hai là một cây phân nhánh có các chuỗi từ lá đến cành đều có độ dài chẵn. Ví dụ,```
1
5
1 1 2 3
```có gốc 1 với con 2 và 3, trong khi 2 có con 4 và 3 có con 5. Hai chuỗi lá là (4\rightarrow2) và (5\rightarrow3), mỗi chuỗi chứa hai đỉnh trước khi đến gốc nhánh. Câu trả lời đúng là`Meiya`. Một giải pháp dựa trên độ sâu từ gốc đến lá thông thường sẽ không nắm bắt được sự phân rã có liên quan vì hai nhánh có chung gốc. 

Một chuỗi thuần túy là một trường hợp ranh giới khác. Vì```
1
3
1 2
```có chính xác một lá có sẵn ở mỗi lượt, vì vậy trò chơi kéo dài ba nước đi và câu trả lời là`Takeru`. Đối với bốn đỉnh,```
1
4
1 2 3
```trò chơi kéo dài bốn nước đi và câu trả lời là`Meiya`. Đây là lý do tại sao chiều dài chuỗi phải bao gồm cả chiếc lá và khi toàn bộ cây là một chuỗi thì cũng bao gồm cả gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi vị trí như một trò chơi khách quan và liệt kê đệ quy mọi nước đi có thể. Nếu cây hiện tại có (k) lá thì có (2^k-1) tập con lá khác trống có thể bị loại bỏ. Chúng ta có thể đánh giá đệ quy từng cây kết quả và ghi nhớ trạng thái chiến thắng của nó. Điều này đúng vì một thế cờ đang thắng chính xác khi có ít nhất một nước đi hợp pháp dẫn đến thế thua. 

Vấn đề là số lượng lựa chọn. Xét một ngôi sao (n)-đỉnh, với gốc được kết nối trực tiếp với tất cả (n-1) đỉnh khác. Người chơi đầu tiên đã có 

[ 
2^{n-1}-1 
] 

động thái pháp lý khác nhau. Do đó, ngay cả cấp độ đầu tiên của tìm kiếm toàn diện cũng có tính cấp số nhân. Việc triển khai không gian trạng thái chung cũng có thể yêu cầu bộ nhớ hàm mũ, vì vậy phương pháp này không thể sử dụng được với (n=10^6). 

Quan sát quan trọng là các đỉnh phân nhánh sẽ tách cây thành các chuỗi đỉnh độc lập với đúng một con. Xét một chiếc lá và di chuyển lên trên sao cho mỗi đỉnh gặp đều có đúng một đỉnh con. Đường dẫn tối đa thu được là một chuỗi có thông tin có ý nghĩa duy nhất là độ dài tương đương của nó. Trò chơi trở nên đặc biệt đơn giản khi chúng ta phân biệt xem một chiếc lá có anh em hay không. 

Giả sử một lá (x) nào đó có lá mẹ có ít nhất hai lá con. Khi đó vị trí sẽ giành chiến thắng ngay cho người chơi di chuyển. Xóa (x). Nếu vị trí kết quả là thua, chúng ta kết thúc. Nếu nó đang thắng, hãy lấy nước đi thắng từ vị trí kết quả đó và thêm (x) vào nước đi tương tự. Việc loại bỏ (x) không thể tạo ra một lá khác ở nơi khác, bởi vì cha mẹ của nó vẫn còn một lá con khác, vì vậy mọi đỉnh được sử dụng bởi nước đi thắng đó đã là một lá trước khi (x) bị loại bỏ. Chúng ta lại rơi vào thế thua trong một nước đi ban đầu. Đây là bổ đề cấu trúc trung tâm. 

Sau khi loại trừ trường hợp này, không có lá nào có lá anh em. Mỗi lá thuộc một chuỗi cực đại mà mỗi đỉnh trong của nó có đúng một con. Xác định độ dài chuỗi là số đỉnh từ lá trở lên, dừng trước tổ tiên đầu tiên có ít nhất hai con. Nếu toàn bộ cây là một chuỗi thì gốc được coi là phần cuối của chuỗi. 

Bây giờ tính chẵn lẻ hoàn toàn quyết định kết quả. Nếu chuỗi nào đó có độ dài lẻ, người chơi đầu tiên có thể loại bỏ lá ở cuối mỗi chuỗi có độ dài lẻ trong một nước đi. Vì không có lá nào có anh chị em nên mỗi chuỗi như vậy có chiều dài ít nhất là hai trừ khi toàn bộ cây là một chuỗi. Việc loại bỏ lá của nó sẽ thay đổi chiều dài lẻ thành chiều dài chẵn mà không để lộ đỉnh phân nhánh. Vị trí kết quả chỉ có chuỗi chẵn. 

Nếu mọi chuỗi đều chẵn thì vị trí sẽ bị mất. Bất kỳ động thái nào cũng sẽ loại bỏ lá khỏi ít nhất một chuỗi, thay đổi mọi chuỗi bị ảnh hưởng từ chẵn sang lẻ. Nếu một chuỗi bị ảnh hưởng có chiều dài bằng một, thì lá của nó hiện có nhánh gốc phân nhánh, ngay lập tức mang lại cho người chơi tiếp theo trường hợp cấu trúc chiến thắng. Ngược lại, chuỗi lẻ vẫn còn và chuỗi lẻ tự nó là một vị thế chiến thắng. Như vậy, mỗi nước đi từ thế cân bằng đều mang lại cho đối thủ một thế thắng. 

Điều này mang lại một tiêu chí đặc biệt nhỏ: câu trả lời là`Takeru`nếu một lá nào đó có lá mẹ có nhiều hơn một lá con, hoặc nếu một chuỗi lá cực đại nào đó có độ dài lẻ. Nếu không thì câu trả lời là`Meiya`. Đặc tính này và cách triển khai theo thời gian tuyến tính cũng được đưa ra trong các bản ghi giải pháp cuộc thi độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n)) trong bảng liệt kê trạng thái chung, với (\Omega(2^{n-1})) di chuyển đầu tiên trên một ngôi sao | (O(2^n)) với khả năng ghi nhớ | Quá chậm | 
| Tối ưu | (O(n)) trên mỗi trường hợp thử nghiệm, (O(\sum n)) tổng thể | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cha của mỗi đỉnh và đếm số con của mỗi đỉnh. Số lượng con cho chúng ta biết ngay một đỉnh có phải là điểm phân nhánh hay không. 
2. Quét tất cả các đỉnh không có con nào. Đây chính xác là những chiếc lá hiện tại của cây ban đầu. Đối với mỗi lá (v), trước tiên hãy kiểm tra lá mẹ của nó. Nếu cha mẹ có ít nhất hai con, hãy trả về`Takeru`ngay lập tức vì bổ đề chiến thắng về cấu trúc được áp dụng. 
3. Nếu cha mẹ có đúng một con, hãy đi lên trên trong chuỗi trong khi cha mẹ của đỉnh hiện tại vẫn có đúng một con. Bắt đầu độ dài chuỗi ở mức 1 vì bản thân chiếc lá cũng thuộc về chuỗi. Mỗi lần chúng ta di chuyển đến phần tử mẹ của nó, hãy tăng độ dài lên 1. 
4. Dừng khi đỉnh hiện tại là gốc hoặc đỉnh cha của nó là đỉnh phân nhánh. Đỉnh phân nhánh không phải là một phần của chuỗi này, trong khi gốc được bao gồm nếu bản thân cây là một chuỗi đơn. 
5. Nếu độ dài chuỗi này là số lẻ, hãy trả về`Takeru`. Việc loại bỏ lá ở cuối mỗi chuỗi lẻ sẽ mang lại cho đối thủ một vị trí mà tất cả các độ dài chuỗi đều bằng nhau, tức là thua. 
6. Nếu mỗi lá tạo ra một chuỗi chẵn và không có lá nào có lá mẹ phân nhánh, hãy trả về`Meiya`. Mỗi nước đi có thể tạo ra một chuỗi lẻ hoặc một lá mà cha mẹ đang phân nhánh, vì vậy người chơi tiếp theo luôn nhận được vị trí chiến thắng. 

Lý do bước đi lên vẫn tuyến tính là vì chúng ta chỉ đi qua các đỉnh có đúng một con. Một đỉnh như vậy chỉ thuộc một chuỗi lá nên hai lá khác nhau không thể đi qua nó. Các đỉnh phân nhánh kết thúc bước đi. Do đó, mặc dù việc triển khai dường như chứa một vòng lặp lồng nhau, nhưng tổng số lần lặp trên tất cả các lá là (O(n)). 

### Tại sao nó hoạt động 

Có hai loại vị trí chiến thắng. Lá đầu tiên chứa một lá mà cha mẹ có ít nhất hai con. Người chơi đầu tiên luôn có thể kết hợp lá đó vào một nước đi dẫn đến thế thua. Chuỗi thứ hai chứa chuỗi lá tối đa có chiều dài lẻ. Việc loại bỏ các lá cuối của tất cả các chuỗi lẻ sẽ thay đổi mọi độ dài chuỗi có liên quan từ lẻ sang chẵn, tạo ra vị thế thua. 

Khi không có điều kiện chiến thắng nào tồn tại, mọi chuỗi lá tối đa đều chẵn và mỗi lá có một chuỗi cha mẹ không có anh em duy nhất. Bất kỳ nước đi nào cũng làm thay đổi ít nhất một chuỗi chẵn thành chuỗi lẻ. Nếu chuỗi giảm xuống còn một độ dài thì đỉnh gốc của nó là đỉnh phân nhánh và điều kiện thắng về cấu trúc xuất hiện. Ngược lại, chuỗi lẻ vẫn còn trực tiếp. Như vậy, mọi nước đi từ vị trí chẵn đều dẫn đến vị trí thắng, khiến vị trí ban đầu bị thua. Điều này thiết lập chính xác tiêu chí được sử dụng bởi thuật toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        parent = [0] * (n + 1)
        child_count = [0] * (n + 1)

        data = list(map(int, input().split()))

        for i, p in enumerate(data, 2):
            parent[i] = p
            child_count[p] += 1

        winning = False

        for v in range(2, n + 1):
            if child_count[v] != 0:
                continue

            p = parent[v]

            # A leaf with a sibling is an immediate winning position.
            if child_count[p] > 1:
                winning = True
                break

            # Count vertices in the maximal degree-1 chain.
            length = 1
            u = v

            while u != 1 and child_count[parent[u]] == 1:
                u = parent[u]
                length += 1

            if length & 1:
                winning = True
                break

        out.append("Takeru" if winning else "Meiya")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`parent`mảng lưu trữ phần tử cha của mỗi đỉnh, trong khi`child_count`ghi số lượng trẻ em. Không cần danh sách kề vì thuật toán chỉ đi lên từ các lá. 

Vòng lặp bắt đầu ở đỉnh 2 vì đỉnh 1 là gốc và (n\ge2), do đó gốc ban đầu không thể là lá. Đối với mỗi chiếc lá,`child_count[p] > 1`xử lý điều kiện chiến thắng ngay lập tức trước bất kỳ quá trình truyền tải chuỗi nào. 

Độ dài chuỗi bắt đầu từ 1 vì bản thân chiếc lá là một phần của chuỗi. Kiểm tra vòng lặp`u != 1`trước khi truy cập`parent[u]`, để tránh trường hợp đặc biệt là gốc không có cha. Khi đạt đến gốc, nó đã được tính, điều này cho kết quả chính xác về một chuỗi thuần túy. Ví dụ: chuỗi ba đỉnh tạo ra độ dài 3 và đang thắng, trong khi chuỗi bốn đỉnh tạo ra độ dài 4 và đang thua. 

điều kiện`child_count[parent[u]] == 1`có nghĩa là cha mẹ vẫn ở trong chuỗi con đơn hiện tại. Nếu số con của nó lớn hơn một thì đỉnh cha đó là đỉnh phân nhánh và không được đưa vào. Ranh giới này là nguồn gốc của nhiều lỗi riêng lẻ trong bài toán này. 

Số nguyên Python không có vấn đề tràn ở đây. Mỗi giá trị được lưu trữ tối đa là (10^6) và thuật toán chỉ thực hiện so sánh số nguyên, số gia và truy cập mảng. 

Dòng đầu vào chứa (n-1) cha có thể chứa tới gần một triệu số nguyên. Chuyển đổi dòng đó một lần với`map`Và`list`giữ cho việc phân tích cú pháp đơn giản và tránh gọi nhiều lần`input()`cho các đỉnh riêng lẻ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm mẫu đầu tiên là```
1
3
1 1
```Cây là một chuỗi (1\rightarrow2\rightarrow3). Có một lá, đỉnh 3. 

| Lá | Đếm con cái cha mẹ | Truyền tải chuỗi | Chiều dài chuỗi | Kết quả | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | (3\rightarrow2\rightarrow1) | 3 | Takeru | 

Lá bố mẹ có đúng một lá con nên điều kiện phân nhánh ngay lập tức không áp dụng. Việc duyệt bao gồm các đỉnh 3, 2 và 1, cho độ dài lẻ. Takeru có thể loại bỏ đỉnh 3, sau đó Meiya phải loại bỏ đỉnh 2 và Takeru loại bỏ gốc. 

### Mẫu 2 

Trường hợp thử nghiệm mẫu thứ hai là```
1
4
1 2 3
```Đây là chuỗi bốn đỉnh (1\rightarrow2\rightarrow3\rightarrow4). 

| Lá | Đếm con cái cha mẹ | Truyền tải chuỗi | Chiều dài chuỗi | Kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | (4\rightarrow3\rightarrow2\rightarrow1) | 4 | Meiya | 

Một lần nữa không có lá phân nhánh. Chuỗi hoàn chỉnh có bốn đỉnh nên chiều dài của nó là chẵn. Người chơi đầu tiên không có lựa chọn nào khác ngoài việc loại bỏ từng đỉnh một và người chơi thứ hai loại bỏ gốc. 

Hai ví dụ này chứng minh tại sao gốc phải được tính trong một chuỗi thuần túy. Việc đếm các cạnh thay vì các đỉnh sẽ đảo ngược tính chẵn lẻ và đưa ra câu trả lời sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) trên mỗi trường hợp thử nghiệm, (O(\sum n)) tổng thể | Việc xây dựng số lượng con là tuyến tính và tất cả các đỉnh của chuỗi con đơn đều được duyệt tối đa một lần | 
| Không gian | (O(n)) | Mỗi mảng đếm cha và đếm con đều chứa (n+1) số nguyên | 

Tổng số đỉnh trong tất cả các trường hợp thử nghiệm nhiều nhất là (10^6), do đó quá trình tính toán hoàn chỉnh chỉ thực hiện một lượng công việc không đổi trên mỗi đỉnh. Không có đệ quy, điều này cần thiết cho cây có độ sâu (10^6) và không có biểu diễn trạng thái trò chơi theo cấp số nhân. Kho lưu trữ chính thức đưa ra giới hạn thời gian dự thi là một giây và giới hạn bộ nhớ lớn, khiến việc mô tả đặc tính thời gian tuyến tính trở thành quy mô dự kiến ​​của giải pháp. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    t = next(it)
    answers = []

    for _ in range(t):
        n = next(it)

        parent = [0] * (n + 1)
        child_count = [0] * (n + 1)

        for v in range(2, n + 1):
            p = next(it)
            parent[v] = p
            child_count[p] += 1

        winning = False

        for v in range(2, n + 1):
            if child_count[v] != 0:
                continue

            p = parent[v]

            if child_count[p] > 1:
                winning = True
                break

            length = 1
            u = v

            while u != 1 and child_count[parent[u]] == 1:
                u = parent[u]
                length += 1

            if length & 1:
                winning = True
                break

        answers.append("Takeru" if winning else "Meiya")

    return "\n".join(answers)

# Provided samples
assert solution(
    "2\n"
    "3\n"
    "1 1\n"
    "4\n"
    "1 2 3\n"
) == "Takeru\nMeiya", "provided samples"

# Minimum-size tree: 1 -> 2, exactly two vertices.
assert solution(
    "1\n"
    "2\n"
    "1\n"
) == "Meiya", "minimum-size chain"

# Three-vertex chain: odd number of vertices.
assert solution(
    "1\n"
    "3\n"
    "1 2\n"
) == "Takeru", "odd chain"

# Star: every parent value is 1, so a leaf has a branching parent.
assert solution(
    "1\n"
    "5\n"
    "1 1 1 1\n"
) == "Takeru", "all-equal parents"

# Two even branches:
#       1
#      / \
#     2   3
#     |   |
#     4   5
assert solution(
    "1\n"
    "5\n"
    "1 1 2 3\n"
) == "Meiya", "all maximal chains are even"

# Maximum-size star, also stresses the largest allowed n and repeated parents.
n = 1_000_000
max_case = "1\n" + str(n) + "\n" + ("1 " * (n - 2)) + "1\n"
assert solution(max_case) == "Takeru", "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 3 / 1 1 / 4 / 1 2 3`|`Takeru`,`Meiya`| Mẫu chính thức và tính chẵn lẻ của chuỗi | 
|`1 / 2 / 1`|`Meiya`| Cây tối thiểu và chuỗi chẵn nhỏ nhất | 
|`1 / 3 / 1 2`|`Takeru`| Chuỗi lẻ nhỏ nhất và đếm gốc | 
|`1 / 5 / 1 1 1 1`|`Takeru`| Giá trị gốc hoàn toàn bằng nhau và điều kiện gốc phân nhánh | 
|`1 / 5 / 1 1 2 3`|`Meiya`| Nhiều nhánh có độ dài chuỗi bằng nhau | 
| Ngôi sao có kích thước tối đa với (n=10^6) |`Takeru`| Ràng buộc tối đa, giá trị gốc lặp lại và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với cây tối thiểu```
1
2
1
```đỉnh 2 là lá duy nhất và đỉnh 1 có đúng một con. Quá trình duyệt chuỗi đếm đỉnh 2 rồi đến đỉnh 1, tạo ra độ dài 2. Vì độ dài chẵn và không có lá phân nhánh nên câu trả lời là`Meiya`. Người chơi đầu tiên loại bỏ đỉnh 2 và người chơi thứ hai loại bỏ gốc. 

Đối với trường hợp cha mẹ phân nhánh```
1
4
1 1 2
```đỉnh 3 và 4 là lá. Đỉnh 3 có cha 1, gốc 1 có 2 con nên thuật toán trả về ngay`Takeru`. Một động thái chiến thắng cụ thể là loại bỏ các đỉnh 3 và 4 cùng nhau. Điều này để lại chuỗi hai đỉnh (1\rightarrow2), khiến người chơi tiếp theo bị mất vị trí. Ví dụ này chứng tỏ tại sao điều kiện phân nhánh phải được kiểm tra trước khi thực hiện chuỗi chẵn lẻ. 

Đối với chuỗi lẻ thuần túy```
1
3
1 2
```lá duy nhất là đỉnh 3. Cha mẹ của nó có một con nên thuật toán tuân theo (3\rightarrow2\rightarrow1), đếm ba đỉnh. Kết quả kỳ lạ mang lại`Takeru`. Gốc được bao gồm vì không có tổ tiên phân nhánh nào kết thúc chuỗi. 

Đối với chuỗi chẵn thuần túy```
1
4
1 2 3
```cùng một quá trình duyệt cho ra (4\rightarrow3\rightarrow2\rightarrow1), với độ dài 4. Kết quả là`Meiya`. Điều này mắc phải lỗi phổ biến là đếm cạnh thay vì đếm đỉnh. Có ba cạnh nhưng bốn đỉnh và kết quả trò chơi tuân theo số đỉnh. 

Đối với trường hợp nhánh chẵn```
1
5
1 1 2 3
```các lá là 4 và 5. Lá 4 theo sau (4\rightarrow2) và dừng lại vì cha mẹ của nó, đỉnh 1, có nhiều con. Độ dài chuỗi của nó là 2. Lá 5 tương tự cho độ dài 2. Không có lá nào có lá mẹ phân nhánh và mọi chuỗi đều chẵn, do đó thuật toán in ra`Meiya`. 

Đối với ngôi sao có kích thước tối đa, mọi giá trị gốc là 1:```
1
1000000
1 1 1 ... 1
```Mỗi đỉnh không có gốc là một lá, trong khi gốc 1 có (999999) con. Lá đầu tiên được kiểm tra đã thỏa mãn điều kiện phân nhánh-cha nên thuật toán có thể dừng mà không đi qua bất kỳ chuỗi nào. Kết quả là`Takeru`. Trường hợp này thực hiện cả hành vi thoát tối đa (n) và thoát sớm giúp duy trì việc triển khai hiệu quả ngay cả khi số lượng lá rất lớn.
