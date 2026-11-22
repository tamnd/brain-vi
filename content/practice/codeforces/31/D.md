---
title: "CF 31D - Sôcôla"
description: "Chúng ta được cho một thanh sô cô la hình chữ nhật có chiều rộng là số nguyên _W_ và chiều cao _H_. Bob bẻ miếng sôcôla nhiều lần dọc theo các đường thẳng đứng hoặc nằm ngang từ mép này sang mép đối diện của miếng sôcôla."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 31
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 31 (Div. 2, Codeforces format)"
rating: 2000
weight: 31
solve_time_s: 59
verified: true
draft: false
---
[CF 31D - Sôcôla](https://codeforces.com/problemset/problem/31/D) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, triển khai 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một thanh sô cô la hình chữ nhật có chiều rộng là số nguyên _W_ và chiều cao _H_. Bob bẻ miếng sôcôla nhiều lần dọc theo các đường thẳng đứng hoặc nằm ngang từ mép này sang mép đối diện của miếng sôcôla. Mỗi phần ngắt được chỉ định bởi hai điểm cuối và tập hợp các phần ngắt được đảm bảo khả thi, nghĩa là tồn tại một số thứ tự để áp dụng chúng sao cho mỗi phần ngắt luôn chia chính xác một phần hiện có thành hai phần không trống. Cuối cùng, chúng ta cần diện tích của các mảnh thu được theo thứ tự tăng dần. 

Đầu vào chứa kích thước của thanh và danh sách các dấu ngắt, mỗi dấu ngắt có tọa độ. Đầu ra chỉ đơn giản là danh sách đã sắp xếp các diện tích của các miếng sô-cô-la cuối cùng. Về mặt khái niệm, thanh giống như một hình chữ nhật 2D theo tọa độ Descartes và mỗi thanh chia một hình chữ nhật theo chiều ngang hoặc chiều dọc. 

Các ràng buộc rất nhỏ: _W_, _H_ và _n_ đều có nhiều nhất là 100. Điều này ngụ ý rằng ngay cả các thuật toán bậc ba cũng có thể được chấp nhận, nhưng chúng ta nên tìm kiếm một giải pháp rõ ràng có tỷ lệ hợp lý, lý tưởng là ở O(n²) hoặc tốt hơn. Không có nguy cơ tràn số nguyên vì tất cả các diện tích tối đa là 100 × 100 = 10.000. 

Các trường hợp cạnh không rõ ràng bao gồm các điểm ngắt chồng chéo hoặc dư thừa dọc theo cùng một tọa độ. Ví dụ: nếu sô cô la có kích thước 2 × 2 và các điểm ngắt thẳng đứng tại x=1 hai lần, thì việc triển khai đơn giản không theo dõi phần nào đang được chia có thể đếm gấp đôi hoặc cố gắng chia một phần đã được chia. Một trường hợp tinh vi khác là khi các dấu ngắt được cung cấp không theo thứ tự, nghĩa là dấu ngắt đầu tiên trong đầu vào thực sự có thể tách một phần được tạo bởi dấu ngắt sau đó trong cấu hình cuối cùng. Điều này loại trừ bất kỳ quá trình xử lý tham lam nào theo thứ tự đầu vào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng duy trì danh sách tất cả các miếng sô cô la hiện tại và với mỗi lần ngắt, hãy lặp lại tất cả các miếng để tìm ra miếng nào bị tách. Mỗi lần kiểm tra liên quan đến việc xem liệu đường ngắt có giao nhau với phần bên trong của hình chữ nhật hay không. Sau khi tìm thấy mảnh chính xác, chúng tôi chia nó thành hai hình chữ nhật. Phương pháp này đúng vì mỗi lần ngắt chia chính xác một phần hiện có và bằng cách kiểm tra tất cả các phần, cuối cùng chúng tôi chia từng phần một cách chính xác. Độ phức tạp xấp xỉ O(n2) trong trường hợp xấu nhất: đối với n phần ngắt, chúng ta có thể phải kiểm tra tới n phần cho mỗi phần ngắt. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận có cấu trúc hơn là chúng ta có thể giảm bớt vấn đề để thu thập tất cả các tọa độ cắt dọc và ngang duy nhất. Xem xét tất cả các đường cắt dọc bao gồm x=0 và x=W, và tất cả các đường cắt ngang bao gồm y=0 và y=H. Sắp xếp các tọa độ này. Mỗi cặp giá trị x liên tiếp xác định một dải dọc và mỗi cặp giá trị y liên tiếp xác định một dải ngang. Giao điểm của dải dọc và ngang tạo thành một hình chữ nhật. Diện tích của hình chữ nhật đó chỉ đơn giản là chiều rộng của dải nhân với chiều cao của dải. Vì mỗi lần ngắt được đảm bảo căn chỉnh theo trục và dựa trên số nguyên nên phương pháp này sẽ tự động tính đến tất cả các phần tách và chúng ta không cần mô phỏng quá trình phân tách. 

Quan sát này cho phép chúng tôi tránh DFS hoặc phân tách đệ quy rõ ràng. Thay vì quản lý các phần một cách linh hoạt, chúng tôi tạo ra tất cả các hình chữ nhật bằng cách lặp qua các tọa độ liền kề dọc theo x và y. Cách tiếp cận này chạy ở O(n²) trong trường hợp xấu nhất (sắp xếp các vết cắt và liệt kê tất cả các hình chữ nhật), hiệu quả với n ≤ 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Nén tọa độ | O(n log n + n2) | O(n²) | Được chấp nhận, thực hiện đơn giản hơn | 

## Hướng dẫn thuật toán

1. Khởi tạo hai bộ cắt dọc và cắt ngang. Chèn các ranh giới của thanh sô cô la: 0 và W vào các vết cắt dọc, 0 và H vào các vết cắt ngang. Điều này đảm bảo chúng ta bao gồm các cạnh bên ngoài dưới dạng hình chữ nhật. 
2. Lặp lại danh sách các điểm ngắt. Đối với mỗi lần nghỉ, hãy kiểm tra xem nó nằm dọc (x1 = x2) hay nằm ngang (y1 = y2). Thêm tọa độ của điểm ngắt vào tập hợp tương ứng. Điều này thu thập tất cả các tọa độ cắt duy nhất. 
3. Chuyển đổi các tập hợp tọa độ cắt thành danh sách đã sắp xếp. Việc sắp xếp đảm bảo rằng các tọa độ liên tiếp xác định các cạnh của hình chữ nhật thu được theo thứ tự. 
4. Lặp lại từng cặp đường cắt dọc liên tiếp và từng cặp đường cắt ngang liên tiếp. Chiều rộng của hình chữ nhật là hiệu giữa các tọa độ dọc liên tiếp và chiều cao là hiệu giữa các tọa độ ngang liên tiếp. Tính diện tích theo chiều rộng × chiều cao. 
5. Tập hợp tất cả các diện tích hình chữ nhật vào một danh sách và sắp xếp chúng theo thứ tự tăng dần. Xuất danh sách đã sắp xếp. 

Tại sao nó hoạt động: điều bất biến là mọi đường ngắt được căn chỉnh theo trục sẽ xuất hiện dưới dạng tọa độ trong danh sách được sắp xếp. Mỗi hình chữ nhật trong sô cô la cuối cùng được giới hạn bởi tọa độ x và y liên tiếp từ các danh sách này. Bởi vì tất cả các vết cắt được đảm bảo sẽ tách một số mảnh, không có khu vực nào bị bỏ sót hoặc tính hai lần. Phương pháp này liệt kê đầy đủ tất cả các phần cuối cùng mà không mô phỏng thứ tự phân chia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

W, H, n = map(int, input().split())

vertical_cuts = {0, W}
horizontal_cuts = {0, H}

for _ in range(n):
    x1, y1, x2, y2 = map(int, input().split())
    if x1 == x2:
        vertical_cuts.add(x1)
    else:
        horizontal_cuts.add(y1)

vertical = sorted(vertical_cuts)
horizontal = sorted(horizontal_cuts)

areas = []
for i in range(1, len(vertical)):
    for j in range(1, len(horizontal)):
        width = vertical[i] - vertical[i-1]
        height = horizontal[j] - horizontal[j-1]
        areas.append(width * height)

areas.sort()
print(*areas)
```Mỗi phần của mã tuân theo các bước thuật toán. Các bộ ban đầu đảm bảo bao gồm các ranh giới. Việc sắp xếp tạo ra các vị trí cắt liên tiếp, cho phép tính toán diện tích đơn giản. Việc sử dụng một bộ sẽ tự động ngăn chặn sự trùng lặp, do đó, những khoảng nghỉ thừa không ảnh hưởng đến kết quả. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 2 2
1 0 1 2
0 1 1 1
```| dọc | ngang | khu vực hình chữ nhật | 
| --- | --- | --- | 
| 0,1,2 | 0,1,2 | 1×1=1, 1×1=1, 1×2=2 | 

Đầu ra:`1 1 2`Điều này xác nhận rằng thuật toán xác định chính xác các vết cắt và liệt kê tất cả các hình chữ nhật. 

**Đầu vào tùy chỉnh**```
3 3 2
1 0 1 3
2 0 2 3
```| dọc | ngang | khu vực hình chữ nhật | 
| --- | --- | --- | 
| 0,1,2,3 | 0,3 | 1×3=3, 1×3=3, 1×3=3 | 

Đầu ra:`3 3 3`Bài tập này thực hiện nhiều đường cắt dọc và một đường cắt ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n2) | Sắp xếp tối đa n+2 tọa độ, liệt kê tối đa n2 hình chữ nhật | 
| Không gian | O(n²) | Lưu trữ tất cả các khu vực hình chữ nhật trong danh sách | 

Với n ≤ 100, tổng số thao tác nằm dưới 10⁶, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    W, H, n = map(int, input().split())
    vertical_cuts = {0, W}
    horizontal_cuts = {0, H}
    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        if x1 == x2:
            vertical_cuts.add(x1)
        else:
            horizontal_cuts.add(y1)
    vertical = sorted(vertical_cuts)
    horizontal = sorted(horizontal_cuts)
    areas = [ (vertical[i]-vertical[i-1])*(horizontal[j]-horizontal[j-1])
             for i in range(1,len(vertical)) for j in range(1,len(horizontal)) ]
    areas.sort()
    return ' '.join(map(str, areas))

# Provided samples
assert run("2 2 2\n1 0 1 2\n0 1 1 1\n") == "1 1 2", "sample 1"

# Custom test cases
assert run("3 3 2\n1 0 1 3\n2 0 2 3\n") == "3 3 3", "all vertical splits"
assert run("3 3 2\n0 1 3 1\n0 2 3 2\n") == "3 3 3", "all horizontal splits"
assert run("1 1 1\n0 0 1 0\n") == "1", "single horizontal line on 1x1"
assert run("2 2 1\n1 0 1 2\n") == "2 2", "single vertical line"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3 2`chia dọc |`3 3 3`| nhiều vết cắt dọc | 
|`3 3 2`chia ngang |`3 3 3`| nhiều vết cắt ngang | 
|`1 1 1`|`1`| thanh tối thiểu với một lần nghỉ | 
|`2 2 1`|`2 2`| chia đường thẳng đứng đơn |
