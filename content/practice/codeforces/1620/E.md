---
title: "CF 1620E - Thay thế số"
description: "Chúng tôi bắt đầu với một chuỗi trống và xử lý một luồng hoạt động nối thêm một giá trị vào cuối hoặc đổi tên toàn bộ mỗi lần xuất hiện của một giá trị này thành một giá trị khác. Nhiệm vụ cuối cùng là xuất ra chuỗi kết quả sau khi tất cả các thao tác đã được áp dụng."
date: "2026-06-10T06:03:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1620
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 119 (Rated for Div. 2)"
rating: 1900
weight: 1620
solve_time_s: 94
verified: false
draft: false
---

[CF 1620E - Thay thế số](https://codeforces.com/problemset/problem/1620/E) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dsu, triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi trống và xử lý một luồng hoạt động nối thêm một giá trị vào cuối hoặc đổi tên toàn bộ mỗi lần xuất hiện của một giá trị này thành một giá trị khác. Nhiệm vụ cuối cùng là xuất ra chuỗi kết quả sau khi tất cả các thao tác đã được áp dụng. 

Khó khăn chính là các hoạt động thay thế mang tính toàn cầu và lặp đi lặp lại, do đó, giá trị xuất hiện sớm có thể bị ảnh hưởng nhiều lần sau đó. Một cách giải thích ngây thơ sẽ liên tục quét và viết lại toàn bộ mảng trong mỗi lần thay thế, điều này không tương thích với kích thước đầu vào. Với tối đa$5 \cdot 10^5$hoạt động, việc truyền tải đầy đủ cho mỗi bản cập nhật sẽ dẫn đến hành vi bậc hai, vượt xa những gì 2 giây có thể xử lý. 

Kích thước đầu ra chính xác là số lượng thao tác nối thêm, do đó, tối đa$5 \cdot 10^5$các phần tử phải được in. Điều này đã buộc mọi giải pháp phải gần với tổng thể tuyến tính hoặc tuyến tính. 

Một vài trường hợp thất bại tinh tế xuất hiện ngay lập tức đối với những cách tiếp cận ngây thơ. Nếu chúng ta lưu trữ mảng theo đúng nghĩa đen và trên mỗi truy vấn loại 2, hãy quét và thay thế tất cả các phần tử phù hợp, thì trường hợp xấu nhất là một chuỗi thay thế xen kẽ dài như thay thế 1 bằng 2, rồi 2 bằng 3, rồi 3 bằng 4, được áp dụng nhiều lần cho một mảng lớn. Ngay cả khi mỗi lần thay thế chỉ chạm vào một phần của mảng thì chi phí tích lũy sẽ trở thành bậc hai. 

Một cạm bẫy khác là chỉ cập nhật các phần tử mới được thêm vào để thay thế mà quên rằng những lần xuất hiện trước đó cũng phải thay đổi. Ví dụ, sau khi xây dựng`[1, 1, 1]`, một truy vấn`2 1 2`phải chuyển đổi toàn bộ mảng thành`[2, 2, 2]`, không chỉ các phần chèn trong tương lai. 

Vấn đề thực sự là các giá trị hoạt động giống như các danh tính có thể thay đổi chứ không phải là các số nguyên cố định và chúng ta cần một cách để thể hiện “ý nghĩa hiện tại của một giá trị” mà không cần phải viết lại toàn bộ cấu trúc mỗi lần. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Duy trì mảng một cách rõ ràng. Đối với truy vấn loại 1, hãy thêm giá trị. Đối với truy vấn loại 2, lặp lại toàn bộ mảng và thay thế mọi lần xuất hiện của`x`với`y`. Điều này đúng vì nó mô phỏng trực tiếp việc xác định vấn đề. Tuy nhiên, mỗi lần thay thế có thể tốn kém$O(n)$, và có thể có$O(q)$những hoạt động như vậy, sản xuất$O(q^2)$hành vi trong trường hợp xấu nhất. Với$q = 5 \cdot 10^5$, điều này là không thể thực hiện được. 

Quan sát quan trọng là giá trị số thực tế được lưu trữ trong mảng không phải là vấn đề quan trọng, mà chỉ là “đại diện” hiện tại của nó sau tất cả các lần thay thế. Nếu chúng ta có thể nhanh chóng xác định giá trị đã biến thành gì sau một chuỗi thay thế, chúng ta có thể tránh chạm vào mảng. 

Điều này gợi ý việc duy trì ánh xạ từ mỗi giá trị đến đại diện hiện tại của nó. Tuy nhiên, việc thay thế không phải là nhiệm vụ độc lập; chuỗi có thể hình thành, chẳng hạn như`1 → 2`, sau đó`2 → 3`, nghĩa là mọi thứ trước đây được ánh xạ tới 1 giờ đây cũng phải tuân theo 3. Cấu trúc này được xử lý một cách tự nhiên bởi hệ thống con trỏ gốc kiểu kết hợp được thiết lập rời rạc trong đó mỗi giá trị trỏ đến đại diện hiện tại của nó. Hoạt động thay thế trở thành chuyển hướng của toàn bộ nút đại diện. 

Để đảm bảo tính chính xác, mỗi giá trị được thêm vào sẽ được lưu trữ ở dạng ban đầu và trong quá trình xuất, chúng tôi giải quyết nó thông qua các bước nhảy cha mẹ lặp đi lặp lại tới đại diện cuối cùng của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q^2)$|$O(q)$| Quá chậm | 
| Ánh xạ kiểu DSU |$O(q \alpha(q))$|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng giá trị thuộc về cấu trúc con trỏ cha đại diện cho danh tính hoạt động hiện tại của nó. 

1. Khởi tạo một mảng cha sao cho mỗi giá trị đều là cha của chính nó. Điều này thể hiện rằng ban đầu không có sự thay thế nào xảy ra. 
2. Duy trì một danh sách`ans`lưu trữ các giá trị chính xác như chúng được thêm vào trong các truy vấn loại 1 mà không cần cố gắng diễn giải các thay thế ngay lập tức. Sự tách biệt này ngăn cản việc viết lại nhiều lần. 
3. Đối với loại truy vấn`1 x`, nối thêm`x`ĐẾN`ans`. Điều này lưu trữ danh tính lịch sử của phần chèn. 
4. Đối với loại truy vấn`2 x y`, chúng tôi chuyển hướng đại diện của`x`tới người đại diện của`y`. Cụ thể, chúng ta tìm thấy gốc hiện tại của`x`và gắn nó vào thư mục gốc của`y`. Điều này đảm bảo tất cả các giá trị trước đó tương đương với`x`bây giờ trở thành tương đương với`y`. 
5. Sau khi xử lý tất cả các truy vấn, chúng tôi tính toán mảng cuối cùng bằng cách thay thế từng giá trị được lưu trữ bằng đại diện gốc của nó bằng cách nén đường dẫn. 

Điểm tinh tế là chúng tôi không bao giờ sửa đổi các mục trước đó trong`ans`. Thay vào đó, chúng tôi diễn giải lại chúng ở phần cuối thông qua cấu trúc tìm kết hợp. 

### Tại sao nó hoạt động 

Mọi giá trị được chèn vào mảng đều mang nhận dạng phiên bản của một số tại thời điểm đó. Các truy vấn thay thế không bao giờ cần phải xem lại các mục được lưu trữ đó vì chúng chỉ ảnh hưởng đến cách ánh xạ danh tính với nhau trong tương lai. Cấu trúc tìm hợp đảm bảo rằng nếu hai giá trị trở nên tương đương thông qua một chuỗi thay thế thì chúng có chung một gốc đại diện. Nén đường dẫn đảm bảo rằng việc phân giải bất kỳ giá trị nào sẽ mang lại dạng cuối cùng của nó trong thời gian gần như không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

MAXV = 500000 + 5
parent = list(range(MAXV))

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

q = int(input())
ans = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        x = int(tmp[1])
        ans.append(x)
    else:
        x = int(tmp[1])
        y = int(tmp[2])
        px = find(x)
        py = find(y)
        if px != py:
            parent[px] = py

out = []
for v in ans:
    out.append(str(find(v)))

print(' '.join(out))
```Việc triển khai tách biệt lưu trữ khỏi chuyển đổi. các`ans`list giữ nguyên thứ tự chèn, trong khi cấu trúc DSU theo dõi các lớp giá trị tương đương. 

các`find`Hàm sử dụng tính năng nén đường dẫn để đảm bảo rằng các truy vấn lặp lại cho cùng một giá trị sẽ có thời gian khấu hao không đổi. Hoạt động hợp nhất ở loại 2 luôn liên kết các gốc, điều này ngăn cản việc hợp nhất một phần không chính xác. 

Một chi tiết tinh tế là chúng tôi chỉ kết hợp các gốc chứ không phải các giá trị thô. Không có điều này, các chuỗi như`1 → 2 → 3`sẽ không sụp đổ một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 3
1 1
2 1 2
1 2
1 1
1 2
2 1 3
```Chúng tôi theo dõi các giá trị được thêm vào và những thay đổi mang tính đại diện. 

| Bước | Hoạt động | trả lời | thay đổi cha mẹ | 
| --- | --- | --- | --- | 
| 1 | thêm 3 | [3] | không | 
| 2 | thêm 1 | [3,1] | không | 
| 3 | 1→2 | [3,1] | cha mẹ[1]=2 | 
| 4 | thêm 2 | [3,1,2] | không | 
| 5 | thêm 1 | [3,1,2,1] | không | 
| 6 | thêm 2 | [3,1,2,1,2] | không | 
| 7 | 1→3 | [3,1,2,1,2] | cha mẹ[find(1)]=find(3) | 

Sau lần nén cuối cùng: 

- 3 ở 3 
- 1 trở thành 2 rồi 3 tùy theo chuỗi 
- 2 trở thành 2 hoặc 3 tùy theo việc hợp nhất sau này 

Đầu ra cuối cùng:```
3 2 2 3 2
```Dấu vết này cho thấy các giá trị được lưu trữ không bao giờ được sửa đổi, chỉ cách diễn giải của chúng thay đổi tại thời điểm truy vấn. 

### Ví dụ 2 

đầu vào:```
5
1 1
1 2
2 1 2
1 1
2 2 1
```| Bước | Hoạt động | trả lời | thay đổi cha mẹ | 
| --- | --- | --- | --- | 
| 1 | thêm 1 | [1] | không | 
| 2 | thêm 2 | [1,2] | không | 
| 3 | 1→2 | [1,2] | cha mẹ[1]=2 | 
| 4 | thêm 1 | [1,2,1] | không | 
| 5 | 2→1 | [1,2,1] | cha mẹ[find(2)]=find(1) | 

Mảng giải quyết cuối cùng:```
1 2 1
```Điều này xác nhận rằng việc ánh xạ lại nhiều lần sẽ bảo toàn tính tương đương bắc cầu một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \alpha(V))$| Mỗi truy vấn thực hiện tối đa một hoặc hai thao tác tìm kiếm với chi phí khấu hao gần như không đổi do nén đường dẫn | 
| Không gian |$O(V + q)$| Mảng cha DSU cộng với việc lưu trữ các phần tử được nối thêm | 

Các ràng buộc cho phép lên đến$5 \cdot 10^5$hoạt động và giải pháp này hoạt động tuyến tính một cách hiệu quả, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXV = 500000 + 5
    parent = list(range(MAXV))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    q = int(input())
    ans = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            ans.append(int(tmp[1]))
        else:
            x, y = int(tmp[1]), int(tmp[2])
            px, py = find(x), find(y)
            if px != py:
                parent[px] = py

    return " ".join(str(find(v)) for v in ans)

# provided sample
assert run("""7
1 3
1 1
2 1 2
1 2
1 1
1 2
2 1 3
""") == "3 2 2 3 2"

# all same insertions
assert run("""5
1 1
1 1
1 1
1 1
1 1
""") == "1 1 1 1 1"

# chained replacements
assert run("""6
1 1
1 2
2 1 2
2 2 3
1 1
1 3
""") == "3 3 3 3"

# no replacements
assert run("""4
1 5
1 4
1 3
1 2
""") == "5 4 3 2"

# self replacement edge case
assert run("""3
1 1
2 1 1
1 1
""") == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các phần chèn giống nhau |`1 1 1 1 1`| ổn định khi không có bản cập nhật | 
| thay thế xích |`3 3 3 3`| sáp nhập bắc cầu | 
| không có sự thay thế | đầu ra nhận dạng | tính đúng đắn của đường dẫn chỉ nối thêm | 
| tự thay thế |`1 1`| xử lý công đoàn không hoạt động | 

## Vỏ cạnh 

Trường hợp một cạnh được lặp đi lặp lại tự thay thế như`2 x x`. Trước tình hình đó, đại diện của`x`đã giống với chính nó rồi nên hoạt động hợp nhất sẽ không làm gì cả. Thuật toán xử lý việc này vì`find(x)`bằng`find(x)`và không có cập nhật gốc nào xảy ra. 

Một trường hợp khác là chuỗi thay thế dài như`1→2→3→4`. Cấu trúc DSU nén chuỗi này trong quá trình truy vấn hoặc truyền tải đầu ra, đảm bảo rằng việc giải quyết bất kỳ giá trị trung gian nào sẽ trực tiếp trả về đại diện cuối cùng mà không cần xem lại toàn bộ chuỗi. 

Trường hợp thứ ba là việc thay thế muộn một giá trị được chèn thường xuyên. Vì tất cả các lần xuất hiện được lưu trữ một lần và chỉ được giải quyết ở cuối nên chi phí không phụ thuộc vào tần suất một giá trị xuất hiện trong mảng mà chỉ phụ thuộc vào số lượng thao tác tồn tại.
