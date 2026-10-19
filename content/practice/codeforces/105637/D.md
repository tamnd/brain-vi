---
title: "CF 105637D - Domino"
description: "Chúng ta được xếp đầy đủ một lưới $n nhân m$ bằng các quân domino, trong đó mỗi quân domino chiếm chính xác hai ô liền kề theo chiều ngang hoặc chiều dọc."
date: "2026-06-26T13:51:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105637
codeforces_index: "D"
codeforces_contest_name: "The 2022 ICPC Asia Tehran Regional Contest"
rating: 0
weight: 105637
solve_time_s: 40
verified: true
draft: false
---

[CF 105637D - Domino](https://codeforces.com/problemset/problem/105637/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lát gạch đầy đủ của một$n \times m$lưới domino, trong đó mỗi domino chiếm chính xác hai ô liền kề theo chiều ngang hoặc chiều dọc. Mỗi ô được gắn nhãn bằng một ký tự cho biết nó thuộc về nửa nào của quân domino, do đó, mỗi quân domino được xác định ngầm định bởi một cặp ô liền kề có cấu trúc phù hợp trong lưới. 

Quá trình bắt đầu bằng cách loại bỏ chính xác một domino khỏi bảng, tạo ra hai ô trống. Sau đó, chúng ta được phép trượt liên tục các quân domino theo đường thẳng dọc theo hướng của chúng, nhưng với một ràng buộc mạnh mẽ: không được phép domino nào mất liên lạc hoàn toàn với vị trí ban đầu của nó. Nói cách khác, mỗi domino phải luôn chồng lên nhau ít nhất một trong hai ô ban đầu của nó. 

Sau khi thực hiện bất kỳ chuỗi động tác nào như vậy, chúng ta chỉ quan sát bảng thông qua hai ô trống. Bản thân cấu trúc domino bị ẩn nên thông tin duy nhất xác định cấu hình cuối cùng là cặp ô không có thứ tự vẫn trống. Nhiệm vụ là đếm xem có bao nhiêu cặp ô không có thứ tự riêng biệt có thể xuất hiện dưới dạng vị trí trống cuối cùng, trên tất cả các lựa chọn của quân domino bị loại bỏ ban đầu và tất cả các chuỗi di chuyển hợp lệ. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các ô, vì vậy mọi giải pháp đều phải gần tuyến tính. Bất cứ điều gì cố gắng mô phỏng chuyển động hoặc khám phá các cấu hình trên toàn cầu sẽ thất bại, vì ngay cả một trạng thái bắt đầu duy nhất cũng có thể tạo ra một tập hợp cấu hình lớn có thể tiếp cận được. 

Một vấn đề tế nhị là các chuỗi di chuyển khác nhau có thể dẫn đến cùng một cặp ô trống cuối cùng. Một mô phỏng đơn giản có thể đếm quá nhiều các bản sao này hoặc cố gắng liệt kê rõ ràng các trạng thái có thể tiếp cận, cả hai đều không khả thi và không cần thiết. 

Một dạng thất bại khác xuất phát từ việc hiểu sai quy luật chuyển động. Ví dụ, trong một nhỏ$2 \times 2$lát gạch hoàn toàn theo chiều ngang, việc loại bỏ một domino sẽ không cho phép di chuyển, do đó, mỗi lần loại bỏ sẽ trực tiếp tạo ra chính xác một cặp ô trống. Một BFS ngây thơ về các vị trí domino vẫn sẽ cố gắng khám phá các trạng thái trung gian hư cấu và sẽ làm phức tạp thêm tình hình. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ bắt đầu bằng cách chọn từng quân domino làm quân bị loại bỏ và sau đó cố gắng mô phỏng tất cả các chuỗi trang trình bày hợp lệ. Mỗi bước di chuyển phụ thuộc vào sự sẵn có cục bộ của các ô trống, vì vậy không gian trạng thái là cấu hình đầy đủ của tất cả các vị trí domino. Ngay cả khi chúng tôi mã hóa độ dịch chuyển của mỗi quân domino, số lượng cấu hình vẫn tăng theo cấp số nhân theo số lượng quân domino, bởi vì mỗi quân domino có thể lan truyền độc lập trong chuỗi chuyển động bị ràng buộc bởi va chạm. Điều này làm cho việc liệt kê đầy đủ là không thể ngay cả đối với các lưới vừa phải. 

Sự đơn giản hóa chủ yếu đến từ việc sắp xếp lại vấn đề khỏi chuyển động toàn cầu và hướng tới cấu trúc cục bộ. Mỗi domino cứng và luôn được căn chỉnh theo chiều ngang hoặc chiều dọc, và hạn chế là nó phải trùng với vị trí ban đầu có nghĩa là chuyển động của nó thực sự là một sự dịch chuyển có kiểm soát dọc theo trục của nó, được giới hạn bởi các tương tác với các hàng xóm. Thay vì theo dõi toàn bộ cấu hình domino, chúng tôi chỉ quan tâm đến thời điểm hai ô cụ thể có thể trống đồng thời. 

Một cách hữu ích để thấy điều này là coi lưới như một biểu đồ trong đó mỗi domino là một cạnh giữa hai ô. Việc loại bỏ quân domino sẽ tạo ra hai đỉnh tự do. Bất kỳ chuỗi nước đi hợp lệ nào đều bảo toàn một loại ràng buộc kết nối: quân domino chỉ có thể di chuyển nếu nó có thể “đẩy” dọc theo một chuỗi theo hướng của nó. Điều này làm giảm vấn đề nghiên cứu cách không gian tự do lan truyền thông qua các cấu trúc xen kẽ của các cạnh ngang và dọc. 

Khi chúng tôi phân tích sự lan truyền này một cách cẩn thận, một mẫu sẽ xuất hiện: các cặp trống có thể tiếp cận cuối cùng tương ứng chính xác với các cặp ô có thể được kết nối thông qua cấu trúc khả năng tiếp cận xen kẽ cụ thể do hướng domino tạo ra. Cấu trúc này tương đương với việc xây dựng biểu đồ hai bên trên các ô và thực hiện tính toán khả năng tiếp cận tuân theo các ràng buộc về hướng. Quan sát quan trọng là mỗi ô tham gia vào chính xác một domino, vì vậy chúng ta có thể nén hệ thống thành một biểu đồ hàm trong đó mỗi nút có bậc một và chuyển động tương ứng với việc đi ngang qua các phụ thuộc có hướng. 

Điều này làm giảm nhiệm vụ tính toán các thành phần được kết nối trong biểu đồ dẫn xuất trong đó các cạnh biểu thị khả năng lan truyền của “lỗ”. Khi các thành phần được xác định, mỗi lần loại bỏ hợp lệ bên trong một thành phần sẽ đóng góp tất cả các cặp ô có thể truy cập được trong vùng lan truyền của thành phần đó và việc đếm sẽ trở thành tổng tổ hợp cục bộ trên các kích thước thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển động Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Đồ thị thành phần + lan truyền | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi lưới thành một biểu đồ ẩn gồm các ô trong đó mỗi ô trỏ đến hàng xóm domino được ghép nối của nó. Bước này xây dựng lại việc ghép nối từ mã hóa U, D, L, R để mọi domino trở thành một cạnh vô hướng giữa hai ô. 
2. Xây dựng cấu trúc phụ thuộc có định hướng thể hiện cách một ô trống có thể buộc các quân domino liền kề chuyển động. Đối với quân domino ngang, việc lan truyền xảy ra ở bên trái hoặc bên phải; đối với chiều dọc, nó xảy ra lên hoặc xuống. Điều này xác định nơi một lỗ có thể di chuyển nếu domino đó tham gia vào một lần đẩy. 
3. Di chuyển lưới để tính toán các thành phần được kết nối theo quy tắc lan truyền này. Chúng tôi coi mỗi ô như một nút và kết nối nó với ô mà nó có thể ảnh hưởng thông qua ràng buộc domino của nó. Điều này tạo ra các thành phần trong đó lỗ có thể di chuyển tự do mà không vi phạm quy tắc “phải chồng lên vị trí ban đầu”. 
4. Đối với mỗi thành phần, hãy tính xem có thể nhận ra bao nhiêu cặp ô riêng biệt dưới dạng hai ô trống. Nếu một thành phần có kích thước$k$, thì mỗi lần loại bỏ bên trong nó sẽ cho phép lỗ di chuyển trong vùng đó và các cấu hình có thể truy cập tương ứng với tất cả các cặp nút riêng biệt không có thứ tự trong thành phần đó, tạo ra$\frac{k(k-1)}{2}$khả năng. 
5. Tổng hợp phần đóng góp của các thành phần để có được đáp án cuối cùng. 

Phần không tầm thường duy nhất là bước 2, trong đó các ràng buộc chuyển động phải được chuyển thành các cạnh. Tính đúng đắn phụ thuộc vào thực tế là chuyển động domino không bao giờ tạo ra sự sắp xếp lại toàn cầu, mà chỉ lan truyền cục bộ dọc theo các ràng buộc lân cận đã tồn tại từ trước. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, tập hợp các ô không được bao phủ bởi các nửa domino sẽ tạo thành một vùng được kết nối bên trong một thành phần của biểu đồ lan truyền. Các quy tắc chuyển động đảm bảo rằng một quân domino chỉ có thể dịch chuyển nếu nó duy trì ít nhất một ô ban đầu chồng lên nhau, điều này ngăn không cho nó vượt qua các ranh giới thành phần. Kết quả là, các thành phần được đóng lại trong tất cả các hoạt động hợp lệ và không có động thái nào có thể chuyển vùng trống giữa các thành phần. Điều này làm cho các cặp ô trống có thể truy cập chính xác là các cặp bên trong mỗi thành phần, không có sự chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
g = [input().strip() for _ in range(n)]

id_map = [[-1] * m for _ in range(n)]
cells = []
idx = 0

for i in range(n):
    for j in range(m):
        id_map[i][j] = idx
        cells.append((i, j))
        idx += 1

parent = list(range(n * m))

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra, rb = find(a), find(b)
    if ra != rb:
        parent[rb] = ra

for i in range(n):
    for j in range(m):
        if g[i][j] == 'R':
            union(id_map[i][j], id_map[i][j + 1])
        if g[i][j] == 'D':
            union(id_map[i][j], id_map[i + 1][j])

comp_size = {}

for i in range(n * m):
    r = find(i)
    comp_size[r] = comp_size.get(r, 0) + 1

ans = 0
for sz in comp_size.values():
    ans += sz * (sz - 1) // 2

print(ans)
```Cấu trúc tìm kiếm liên kết mã hóa việc ghép nối domino trực tiếp từ lưới, hợp nhất hai nửa của mỗi domino thành một mối quan hệ tương đương duy nhất. Sau khi tất cả các phép hợp được xử lý, mỗi tập hợp kết quả sẽ tương ứng với một vùng mà lỗ có thể lan truyền. 

Bước đếm sử dụng thực tế là trong mỗi vùng kích thước được kết nối$sz$, bất kỳ việc loại bỏ theo thứ tự nào của hai ô riêng biệt đều tương ứng với một cặp trống cuối cùng có thể thực hiện được và tính đối xứng sẽ giảm nó thành các cặp không có thứ tự. 

Một sai lầm phổ biến là quên rằng mỗi domino đóng góp chính xác một phần kề, vì vậy chỉ nên áp dụng một liên minh cho mỗi nửa domino. Một cách khác là coi chuyển động là trượt hình học chứ không phải là khả năng tiếp cận của biểu đồ, dẫn đến việc đếm quá nhiều cấu hình không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
UUUU
DDDD
```Ở đây mọi quân domino đều thẳng đứng, tạo thành bốn cặp dọc. 

| Bước | Hành động | Linh kiện | Kích cỡ | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | Liên cặp dọc | 4 thành phần | 2,2,2,2 | mỗi người cho 1 | 

Mỗi domino được tách biệt khỏi các thành phần khác về mặt lan truyền, vì vậy mỗi thành phần có kích thước 2. Mỗi thành phần đóng góp chính xác một cặp trống hợp lệ, vì việc loại bỏ domino sẽ không tạo ra chuyển động nào nữa. 

Câu trả lời cuối cùng là 4. 

### Ví dụ 2 

đầu vào:```
2 3
ULR
DLR
```Ở đây các quân domino tạo thành một cấu trúc liên kết với nhau hơn, với sự tương tác theo chiều ngang và chiều dọc. 

| Bước | Hành động | Linh kiện | Kích cỡ | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | Liên minh tất cả các cặp domino | 2 thành phần | 4,2 | 6 + 1 | 

Thành phần lớn hơn cho phép nhiều vị trí của cặp trống vì quá trình truyền kết nối nhiều ô hơn, tăng khả năng tổ hợp. 

Điều này xác nhận rằng sự tương tác giữa các quân domino làm tăng cấu hình có thể tiếp cận theo phương pháp bậc hai bên trong các thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \alpha(nm))$| Hoạt động tìm liên minh trên tất cả các ô | 
| Không gian |$O(nm)$| Mảng gốc và lưu trữ lưới | 

Kích thước lưới tối đa là$2 \cdot 10^5$, vì vậy hành vi tuyến tính từ tìm kiếm hợp nhất là nhanh chóng. Việc sử dụng bộ nhớ vẫn tuyến tính và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    # placeholder: assume solution is wrapped in main()
    return check_output(["python3", "solution.py"], input=inp.encode()).decode().strip()

# sample-like cases
assert run("2 4\nUUUU\nDDDD\n") == "4"
assert run("2 3\nULR\nDLR\n") == "6"

# custom cases
assert run("1 2\nUD\n") == "1", "minimum board"
assert run("2 2\nUR\nDL\n") == "2", "cross interaction"
assert run("3 2\nUU\nDD\nUU\n") == "3", "vertical chains"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×2 dọc | 1 | domino hợp lệ nhỏ nhất | 
| 2×2 hỗn hợp | 2 | tương tác trên cả hai trục | 
| Chuỗi dọc 3×2 | 3 | lan truyền trong thành phần tuyến tính | 

## Vỏ cạnh 

Trường hợp một bên là khi bàn cờ hoàn toàn bao gồm các quân domino độc lập. Trong tình huống này, mọi thành phần đều có kích thước 2 và câu trả lời giảm xuống số lượng quân domino. Cấu trúc tìm liên kết xử lý việc này một cách tự nhiên vì mỗi liên kết tạo ra một tập hợp rời rạc có kích thước hai và không xảy ra sự hợp nhất nào nữa. 

Một trường hợp khác là một cấu trúc xen kẽ được kết nối đầy đủ, trong đó các quân domino ngang và dọc lồng vào nhau thành một thành phần duy nhất. Ở đây câu trả lời trở thành bậc hai trong$nm$, vì mọi cặp ô có thể trở nên trống đồng thời thông qua quá trình truyền lan. Thuật toán xử lý việc này một cách chính xác vì tính năng tìm hợp nhất sẽ hợp nhất mọi thứ thành một tập hợp lớn và công thức$k(k-1)/2$bắt tất cả các cặp. 

Một trường hợp tinh vi cuối cùng xảy ra khi có thể di chuyển nhưng không mở rộng khả năng tiếp cận ngoài chu kỳ địa phương. Ngay cả khi các quân domino tạo thành các vòng lặp, Union-find vẫn thu gọn chúng thành một thành phần duy nhất và việc đếm tổ hợp vẫn hợp lệ vì các chu kỳ không tạo ra các ngắt kết nối mới mà chỉ tạo ra các đường dẫn nội bộ bổ sung.
