---
title: "CF 105383A - Trang trại chăn nuôi"
description: "Chúng ta được cung cấp một bộ sưu tập động vật, mỗi loài được mô tả bằng tên loài và giá trị ảnh hưởng bằng số. Trong số những loài động vật này, chúng ta phải thành lập một hội đồng lãnh đạo bằng cách chọn bất kỳ tập hợp con nào, nhưng việc lựa chọn bị hạn chế bởi một loài đặc biệt duy nhất gọi là lợn."
date: "2026-06-23T05:24:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105383
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 105383
solve_time_s: 50
verified: true
draft: false
---

[CF 105383A - Trang trại súc vật](https://codeforces.com/problemset/problem/105383/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập động vật, mỗi loài được mô tả bằng tên loài và giá trị ảnh hưởng bằng số. Trong số những loài động vật này, chúng ta phải thành lập một hội đồng lãnh đạo bằng cách chọn bất kỳ tập hợp con nào, nhưng việc lựa chọn bị hạn chế bởi một loài đặc biệt duy nhất gọi là lợn. 

Hội đồng phải tuân theo hai quy tắc. Đầu tiên, tối đa một con lợn được phép tham gia hội đồng. Thứ hai, nếu bao gồm một con lợn thì mọi con vật không phải con lợn trong hội đồng đều phải có ảnh hưởng nhỏ hơn con lợn đó. Nếu không có lợn thì không có ràng buộc nào liên quan đến lợn được áp dụng, nhưng một hội đồng như vậy không bao giờ là tối ưu vì lợn tồn tại và có khả năng chi phối việc lựa chọn. 

Mục tiêu là tối đa hóa tổng giá trị ảnh hưởng của tập hợp con được chọn theo các ràng buộc này. 

Kích thước đầu vào có thể lên tới 100.000 con vật, điều này ngay lập tức loại trừ mọi giải pháp cố gắng liệt kê các tập hợp con hoặc mô phỏng tất cả các hội đồng có thể có. Bất cứ điều gì thậm chí bậc hai trong n sẽ quá chậm vì 10^10 thao tác vượt xa giới hạn 2 giây. Chúng tôi buộc phải thực hiện một chiến lược xử lý dữ liệu theo thời gian gần tuyến tính hoặc n log n, thường liên quan đến việc sắp xếp hoặc tổng hợp tiền tố. 

Một trường hợp tế nhị phát sinh khi tất cả động vật đều là lợn. Trong trường hợp đó, chúng ta được phép chọn tối đa một con lợn nên câu trả lời chỉ đơn giản là mức độ ảnh hưởng tối đa giữa các con lợn chứ không phải tổng. Một cách tiếp cận ngây thơ cho rằng chúng ta luôn có thể tính tổng tất cả các con lợn sẽ thất bại ở đây. Một trường hợp cạnh khác xuất hiện khi con lợn mạnh nhất rất nhỏ so với sự kết hợp của những con không phải lợn, bởi vì ràng buộc buộc chúng ta phải lọc những con không phải lợn dựa trên một ngưỡng, chứ không phải tối đa hóa toàn bộ cả hai nhóm một cách độc lập. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp mang tính bạo lực là xem xét mọi lựa chọn có thể có của con lợn và sau đó chọn tất cả những con lợn không phải là lợn tương thích. Đối với mỗi con lợn, chúng ta có thể tính toán mức độ ảnh hưởng của nó và sau đó cộng tất cả những con không phải là lợn có mức độ ảnh hưởng nhỏ hơn con lợn đó. Điều này đúng vì một khi con lợn đã được cố định, chiến lược tối ưu cho những người không phải là lợn là luôn lấy tất cả những cái hợp lệ vì bản thân những người không phải là lợn không có hạn chế nào. 

Tuy nhiên, điều này vẫn yêu cầu quét tất cả những con lợn không phải lợn, điều này dẫn đến hành vi O(n^2) trong trường hợp xấu nhất. Với n lên tới 10^5, điều này trở nên hoàn toàn không khả thi. 

Nhận xét quan trọng là điều duy nhất quan trọng ở một con lợn là giá trị ảnh hưởng của nó. Sau khi chúng tôi sửa một con lợn có ảnh hưởng x, tất cả những con lợn không phải là con lợn có giá trị nhỏ hơn x sẽ tự động đủ điều kiện và tất cả những con lợn có giá trị lớn hơn hoặc bằng đều bị loại trừ. Điều này gợi ý việc sắp xếp những con không phải lợn theo mức độ ảnh hưởng và tính toán trước các tổng tiền tố để chúng ta có thể trả lời “tổng của tất cả những con không phải lợn có giá trị < x” theo thời gian logarit. 

Sau đó, chúng tôi lặp lại tất cả các con lợn, coi mỗi con lợn là ràng buộc tối đa ứng cử viên và kết hợp giá trị của nó với đóng góp không phải con lợn tốt nhất có thể bằng cách sử dụng tìm kiếm nhị phân. Điều này làm giảm vấn đề sắp xếp các tổng tiền tố cộng với tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra lợn bằng cách quét toàn bộ | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + tổng tiền tố + tìm kiếm nhị phân trên lợn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chia tất cả động vật thành hai nhóm tùy theo loài: lợn và không phải lợn. Điều này là cần thiết vì lợn đóng vai trò là điểm neo của ngưỡng trong khi những con không phải lợn chỉ bị hạn chế bởi ngưỡng đó. 
2. Trích xuất các giá trị ảnh hưởng của những thứ không phải lợn vào danh sách và sắp xếp theo thứ tự tăng dần. Việc sắp xếp là cần thiết để tất cả các truy vấn “nhỏ hơn x” đều trở thành vấn đề về tiền tố. 
3. Xây dựng một mảng tổng tiền tố dựa trên các ảnh hưởng không phải của lợn đã được sắp xếp. Mỗi tổng tiền tố tại chỉ số i thể hiện mức độ ảnh hưởng tổng thể của tất cả những người không phải là lợn đối với chỉ số đó. 
4. Sắp xếp các ảnh hưởng của lợn vì chúng tôi sẽ đánh giá từng con lợn như một hạn chế tối đa tiềm năng. 
5. Đối với mỗi con lợn có ảnh hưởng x, hãy thực hiện tìm kiếm nhị phân trên mảng không phải lợn đã được sắp xếp để tìm chỉ số cuối cùng trong đó mức ảnh hưởng hoàn toàn nhỏ hơn x. Điều này xác định chính xác những gì không phải là lợn có thể được đưa vào. 
6. Thêm ảnh hưởng của con lợn x vào tổng tiền tố tại chỉ số đó. Điều này mang lại tổng số tiền tốt nhất có thể cho hội đồng khi con lợn này là thủ lĩnh được chọn. 
7. Theo dõi tối đa trên tất cả các con lợn. Câu trả lời cuối cùng là tổng số tốt nhất có thể đạt được. 

Bước tìm kiếm nhị phân rất quan trọng vì nó thực thi ràng buộc bất đẳng thức một cách hiệu quả mà không cần quét mảng. 

### Tại sao nó hoạt động 

Đối với bất kỳ con lợn cố định nào có ảnh hưởng x, bất kỳ hội đồng hợp lệ nào cũng phải bao gồm con lợn đó và chỉ những con lợn không phải lợn có ảnh hưởng nhỏ hơn x. Trong số tất cả các tập hợp con của những con không phải lợn đó, việc lấy tất cả chúng luôn là tối ưu vì không có hạn chế nội tại nào giữa những con không phải lợn và tất cả các giá trị đều dương. Do đó, giải pháp tối ưu cho mỗi ứng viên lợn sẽ rút gọn thành truy vấn tổng tiền tố. Vì mọi giải pháp hợp lệ phải chọn một số con lợn làm con lợn duy nhất (hoặc tương đương với ràng buộc thống trị), việc liệt kê tất cả các con lợn bao hàm tất cả các khả năng và chúng tôi tận dụng tối đa các bài toán con tối ưu rời rạc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    pigs = []
    non_pigs = []
    
    for _ in range(n):
        sp, val = input().split()
        val = int(val)
        if sp == "pig":
            pigs.append(val)
        else:
            non_pigs.append(val)
    
    if not pigs:
        return print(0)
    
    pigs.sort()
    non_pigs.sort()
    
    prefix = [0]
    for v in non_pigs:
        prefix.append(prefix[-1] + v)
    
    import bisect
    
    ans = 0
    
    for x in pigs:
        idx = bisect.bisect_left(non_pigs, x) - 1
        total = x + (prefix[idx + 1] if idx >= 0 else 0)
        if total > ans:
            ans = total
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là phân vùng đầu vào để lợn và không phải lợn được xử lý độc lập. Danh sách không phải lợn được sắp xếp để chúng tôi có thể xác định một cách hiệu quả những giá trị nào được phép dưới ngưỡng lợn nhất định. Mảng tiền tố biến các truy vấn tổng lặp lại thành tra cứu O(1) sau bước tiền xử lý O(n). 

chức năng`bisect_left(non_pigs, x)`tìm phần tử đầu tiên không nhỏ hơn x, vì vậy trừ đi một phần tử sẽ cho chỉ số hợp lệ cuối cùng nằm dưới x. Chỉ số đó trực tiếp xác định tổng tiền tố của những con không đủ điều kiện. 

Một cạm bẫy triển khai phổ biến là quên mất yêu cầu nghiêm ngặt về bất bình đẳng. sử dụng`bisect_right`thay vào đó sẽ bao gồm không chính xác các giá trị bằng với ảnh hưởng của con lợn, vi phạm ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
pig 10
horse 15
pig 5
cow 20
sheep 25
```Chúng tôi tách lợn như`[10, 5]`và không phải lợn như`[15, 20, 25]`. 

Phân loại cho lợn`[5, 10]`và không phải lợn`[15, 20, 25]`. 

Tổng tiền tố cho những con không phải là lợn là:```
index:   0   1   2   3
value:   0  15  35  60
```Bây giờ đánh giá lợn: 

Với lợn = 5, không có con nào không phải lợn nhỏ hơn 5 nên tổng số là 5. 

Đối với lợn = 10, một lần nữa không có con lợn nào nhỏ hơn 10, nên tổng số là 10. 

Tối đa là 10. 

| lợn | Ngưỡng x | Chỉ số giới hạn trên | Tổng hợp phi lợn | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | -1 | 0 | 5 | 
| 10 | 10 | -1 | 0 | 10 | 

Điều này cho thấy không thể sử dụng những con lợn lớn không phải lợn vì chúng đều vượt quá ngưỡng lợn. 

### Ví dụ 2 

đầu vào:```
pig 10
horse 15
pig 15
cow 15
sheep 10
```Lợn:`[10, 15]`, không phải lợn:`[10, 15, 15]`Tổng tiền tố:```
0, 10, 25, 40
```Đối với lợn = 10, không có con lợn nào hoàn toàn nhỏ hơn 10, nên tổng = 10. 

Đối với lợn = 15, chúng ta chỉ có thể lấy những con không phải lợn nhỏ hơn 15, tức là`[10]`, tổng = 10 nên tổng = 25. 

| lợn | Ngưỡng x | Chỉ số giới hạn trên | Tổng hợp phi lợn | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 10 | 10 | -1 | 0 | 10 | 
| 15 | 15 | 0 | 10 | 25 | 

Con lợn thứ hai chiếm ưu thế vì nó cho phép thêm một con lợn không hợp lệ nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại lợn và không phải lợn cộng với tìm kiếm nhị phân cho mỗi con lợn | 
| Không gian | O(n) | Lưu trữ cho các mảng và tổng tiền tố riêng biệt | 

Các ràng buộc cho phép tối đa 100.000 con vật và giải pháp O(n log n) thực hiện thoải mái trong giới hạn thời gian nhờ khả năng sắp xếp và tìm kiếm nhị phân hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# helper redirect
import sys
old_stdout = sys.stdout

# We redefine run safely
def run(inp: str) -> str:
    import sys, io
    backup_in = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = backup_in
    sys.stdout = backup_out
    return out

# sample-like tests
assert run("""5
pig 10
horse 15
pig 5
cow 20
sheep 25
""") == "10"

assert run("""5
pig 10
horse 15
pig 15
cow 15
sheep 10
""") == "25"

# all pigs
assert run("""3
pig 5
pig 7
pig 3
""") == "7"

# no non-pigs usable
assert run("""4
pig 1
cow 2
horse 3
sheep 4
""") == "1"

# strong pig enables many
assert run("""4
pig 100
cow 10
horse 20
sheep 30
""") == "160"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả lợn | lợn tối đa | hạn chế lợn đơn | 
| lợn thống trị | tổng có ngưỡng | tính chính xác của tiền tố | 
| không có lợn hợp lệ | câu trả lời chỉ có lợn | xử lý bất bình đẳng nghiêm ngặt | 
| trường hợp hỗn hợp | lựa chọn lợn tối ưu | lựa chọn tối đa toàn cầu | 

## Vỏ cạnh 

Khi tất cả động vật đều là lợn, thuật toán vẫn hoạt động vì danh sách không phải lợn trống. Mảng tổng tiền tố chỉ chứa số 0, vì vậy mỗi ứng cử viên lợn sẽ tạo ra tổng bằng giá trị của chính nó. Đối với đầu vào:```
3
pig 5
pig 7
pig 3
```mỗi lần lặp sẽ tính tổng bằng chính con lợn và giá trị chính xác tối đa sẽ là 7. 

Khi tất cả những con không phải lợn đều lớn hơn tất cả những con lợn, mọi tìm kiếm nhị phân đều trả về -1, nghĩa là không có con lợn nào đủ điều kiện. Vì:```
pig 1
cow 10
horse 20
```cả hai con lợn (nếu có nhiều con) sẽ chỉ mang lại giá trị riêng vì phần đóng góp của tiền tố luôn bằng 0. Thuật toán tránh nhầm lẫn việc bao gồm những con không phải là lợn không hợp lệ vì sự bất bình đẳng nghiêm ngặt được thực thi thông qua`bisect_left`. 

Khi các giá trị lợn xen kẽ với các giá trị không phải lợn, độ chính xác phụ thuộc vào việc chọn đúng ngưỡng lợn. Thuật toán đánh giá từng con lợn một cách độc lập, do đó, ngay cả khi một con lợn nhỏ hơn cho phép nhiều con lợn không phải là lợn hơn nhưng con lợn lớn hơn chiếm ưu thế về giá trị, cả hai đều được kiểm tra rõ ràng và mức tối đa được giữ nguyên.
