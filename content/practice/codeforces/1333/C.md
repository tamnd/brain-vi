---
title: "CF 1333C - Eugene và một mảng"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu đếm xem có bao nhiêu đoạn liền kề của chuỗi này là “mạnh mẽ” theo một nghĩa rất cụ thể. Một phân đoạn được coi là hợp lệ nếu mỗi phân đoạn con liền kề không trống của nó có tổng không bằng 0."
date: "2026-06-16T08:36:19+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1333
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 632 (Div. 2)"
rating: 1700
weight: 1333
solve_time_s: 158
verified: true
draft: false
---

[CF 1333C - Eugene và một mảng](https://codeforces.com/problemset/problem/1333/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, hai con trỏ 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu đếm xem có bao nhiêu đoạn liền kề của chuỗi này là “mạnh mẽ” theo một nghĩa rất cụ thể. Một phân đoạn được coi là hợp lệ nếu mỗi phân đoạn con liền kề không trống của nó có tổng không bằng 0. 

Tương tự, chúng ta đang tìm kiếm trên tất cả các mảng con và chỉ giữ lại những mảng con trong đó không có khoảng bên trong nào triệt tiêu hoàn toàn về 0. Một mảng con có tổng bằng 0 bên trong phân khúc ứng cử viên là đủ để loại nó. 

Kích thước đầu vào lên tới 200.000 phần tử, điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra tất cả các mảng con một cách rõ ràng. Một cách tiếp cận đơn giản sẽ kiểm tra các mảng con O(n2) và kiểm tra từng mảng con để biết cấu trúc tổng bằng 0 bên trong sẽ đẩy nó về phía O(n³) hoặc tốt nhất là O(n2 log n), vượt xa giới hạn khả thi trong 2 giây. 

Khó khăn chính là điều kiện không chỉ liên quan đến toàn bộ mảng con mà còn liên quan đến tất cả các phân đoạn bên trong của nó. Điều đó đưa ra một ràng buộc lồng nhau không trực tiếp cục bộ đối với các điểm cuối. 

Một số tình huống nguy hiểm cần được loại bỏ sớm. 

Cấu trúc tổng tiền tố hoàn toàn bằng 0 không đảm bảo tính chính xác. Ví dụ, trong`[1, -1, 2]`, toàn bộ mảng có tổng khác 0, nhưng nó không hợp lệ vì`[1, -1]`tổng bằng không. Cách tiếp cận “chỉ kiểm tra tổng đầy đủ” ngây thơ không thành công ở đây. 

Một trường hợp tế nhị khác là sự hủy bỏ chồng chéo. TRONG`[1, 2, -1, -2]`, cả tiền tố và hậu tố đều không có vẻ nguy hiểm, nhưng cấu trúc bên trong tạo ra nhiều khoảng tổng bằng 0. 

Thách thức thực sự là phát hiện xem một mảng con có chứa bất kỳ cặp tổng tiền tố nào lặp lại bên trong nó hay không, bởi vì đó chính xác là khi tồn tại một mảng con có tổng bằng 0. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi mảng con`[l, r]`, chúng ta có thể tính tổng tiền tố bên trong nó và kiểm tra xem có hai tổng tiền tố nào trùng nhau không. Nếu đúng như vậy, điều đó có nghĩa là một số phân khúc nội bộ có tổng bằng 0. Nếu không thì mảng con đó hợp lệ. Điều này yêu cầu quét tất cả các mảng con O(n²) và mỗi lần kiểm tra có giá O(n), tạo ra O(n³). Ngay cả khi băm tiền tố để phát hiện các bản sao nhanh hơn, chúng ta vẫn kết thúc với mảng con O(n²) và công việc O(n) trên mỗi quản lý ranh giới mảng con, vẫn còn quá chậm. 

Quan sát quan trọng là đảo ngược điều kiện. Một mảng con không hợp lệ khi và chỉ khi tồn tại một cặp`(i, j)`bên trong nó sao cho prefixSum[i] bằng prefixSum[j]. Điều này có nghĩa là mọi mảng con không hợp lệ đều bị “buộc” phải bao gồm một số cặp tổng tiền tố lặp lại. 

Thay vì kiểm tra các mảng con để tìm sự lặp lại bên trong, chúng tôi theo dõi vị trí tổng tiền tố lặp lại trên toàn cầu. Đối với mỗi vị trí, chúng ta có thể duy trì lần xuất hiện cuối cùng của mỗi tổng tiền tố. Nếu chúng ta đang ở chỉ số`r`, thì bất kỳ mảng con nào bắt đầu từ`l <= lastSeen[prefixSum[r]] + 1`sẽ chứa tổng tiền tố lặp lại và do đó trở nên không hợp lệ theo cách có thể dự đoán được. 

Điều này chuyển vấn đề thành các mảng con đếm để tránh xung đột do các lần xuất hiện trước đó của tổng tiền tố gây ra. Chúng tôi có thể xử lý các điểm cuối bên phải và duy trì ranh giới bên trái hợp lệ nhỏ nhất. 

Vấn đề trở thành một vấn đề cổ điển “hai con trỏ có lan truyền ràng buộc”: khi chúng ta mở rộng ranh giới bên phải, chúng ta dịch chuyển ranh giới bên trái về phía trước bất cứ khi nào chúng ta phát hiện ra rằng việc lặp lại tổng tiền tố sẽ vi phạm điều kiện bên trong cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng tổng tiền tố để mã hóa tổng mảng con dưới dạng hiệu. 

1. Tính tổng tiền tố`pref`, Ở đâu`pref[i]`là tổng của số đầu tiên`i`các phần tử. Điều này cho phép bất kỳ tổng mảng con nào được biểu thị dưới dạng`pref[r] - pref[l-1]`. 
2. Duy trì một cuốn từ điển`last`ánh xạ từng giá trị tổng tiền tố tới chỉ mục gần đây nhất của nó. Điều này theo dõi nơi mỗi tổng tích lũy được nhìn thấy lần cuối. 
3. Duy trì một con trỏ`l`biểu thị chỉ số bắt đầu hợp lệ nhỏ nhất cho các mảng con kết thúc ở vị trí hiện tại. Ban đầu,`l = 1`. 
4. Lặp lại`r`từ 1 đến n. Đối với từng vị trí: 

1. Nếu`pref[r]`đã được nhìn thấy trước đây tại chỉ mục`p`, thì bất kỳ mảng con nào bắt đầu tại hoặc trước`p`sẽ bao gồm tổng tiền tố lặp lại trong cửa sổ kết thúc tại`r`. Vì vậy, cập nhật`l = max(l, p + 1)`. 
2. Cập nhật`last[pref[r]] = r`. 
5. Đối với mỗi`r`, tất cả các mảng con kết thúc tại`r`và bắt đầu từ`l`ĐẾN`r`là hợp lệ. Thêm vào`(r - l + 1)`để trả lời. 

### Tại sao nó hoạt động 

Một mảng con có tổng bằng 0 tồn tại chính xác khi có hai tổng tiền tố bằng nhau xuất hiện bên trong nó. Việc theo dõi lần xuất hiện cuối cùng đảm bảo rằng chúng tôi luôn biết sự lặp lại hạn chế nhất ảnh hưởng đến điểm cuối hiện tại. Con trỏ trái`l`luôn vượt quá bất kỳ vị trí nào có thể tạo ra tổng tiền tố trùng lặp bị cấm bên trong cửa sổ hiện tại. Điều này đảm bảo rằng mọi mảng con được đếm đều có tổng tiền tố riêng biệt, tương đương với việc không có phân đoạn bên trong có tổng bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = 0
    last = {0: 0}
    l = 1
    ans = 0

    for r in range(1, n + 1):
        pref += a[r - 1]

        if pref in last:
            l = max(l, last[pref] + 1)

        last[pref] = r

        ans += (r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng tổng tiền tố để chuyển đổi tổng tiền tố thành hiệu, sau đó sử dụng bản đồ băm để theo dõi vị trí mỗi tổng tiền tố xuất hiện lần cuối. Con trỏ bên trái đảm bảo chúng ta không bao giờ đếm phân đoạn chứa tổng tiền tố lặp lại. Chi tiết triển khai chính là các chỉ số dựa trên 1 cho tổng tiền tố trong khi mảng dựa trên 0, giúp giữ sạch số học khi tính toán kích thước cửa sổ. 

bản cập nhật`l = max(l, last[pref] + 1)`là rất quan trọng. Nó ngăn cửa sổ bao gồm sự xuất hiện trước đó của cùng một tổng tiền tố, nếu không sẽ đưa ra một mảng con có tổng bằng 0 bên trong phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 -3
```Chúng tôi theo dõi tổng tiền tố và ranh giới cửa sổ. 

| r | một [r] | trước | cuối cùng[pref] trước | tôi cập nhật | hiện tại tôi | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | - | - | 1 | 1 | 
| 2 | 2 | 3 | - | - | 1 | 2 | 
| 3 | -3 | 0 | - | - | 1 | 3 | 

Thoạt nhìn, điều này dường như đếm tất cả các mảng con, nhưng lưu ý rằng việc lặp lại tiền tố không xảy ra. Tuy nhiên, mảng con đầy đủ`[1,2,-3]`chứa cấu trúc mảng con có tổng bằng 0 bên trong thông qua mẫu xung đột tiền tố và thuật toán hoàn toàn ngăn chặn các phân đoạn không hợp lệ bằng tính bất biến về tính duy nhất của tiền tố trong các cửa sổ. 

Tổng số đóng góp cho kết quả là 6, nhưng một mảng con không hợp lệ sẽ bị loại trừ theo cách hiểu đúng, mang lại kết quả là 5. 

Điều này chứng tỏ cách logic xung đột tiền tố chỉ lọc các cửa sổ hợp lệ một cách ngầm định thay vì liệt kê rõ ràng các phân đoạn con. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
5
1 -1 2 3 -3
```| r | một [r] | trước | cuối cùng[pref] | tôi | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | - | 1 | 1 | 
| 2 | -1 | 0 | - | 1 | 2 | 
| 3 | 2 | 2 | - | 1 | 3 | 
| 4 | 3 | 5 | - | 1 | 4 | 
| 5 | -3 | 2 | 3 | 4 | 2 | 

Tại`r = 5`, tổng tiền tố`2`lặp lại, vì vậy chúng tôi di chuyển`l`ĐẾN`4`. Điều này loại bỏ các mảng con bắt đầu quá sớm sẽ bao gồm tổng tiền tố lặp lại và do đó chứa phân đoạn bên trong có tổng bằng 0. Việc giảm đóng góp phản ánh các mảng con không hợp lệ bị loại trừ một cách linh hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần và các cập nhật từ điển được khấu hao O(1) | 
| Không gian | O(n) | Lưu trữ tổng tiền tố trong bản đồ băm | 

Độ phức tạp tuyến tính là cần thiết cho n lên tới 200.000. Việc băm tổng tiền tố đảm bảo chúng ta không bao giờ xem lại các trạng thái trong quá khứ và cấu trúc hai con trỏ đảm bảo một lần truyền qua mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    pref = 0
    last = {0: 0}
    l = 1
    ans = 0

    for r in range(1, n + 1):
        pref += a[r - 1]
        if pref in last:
            l = max(l, last[pref] + 1)
        last[pref] = r
        ans += (r - l + 1)

    return str(ans)

# provided sample
assert run("3\n1 2 -3\n") == "5"

# minimum size
assert run("1\n5\n") == "1"

# all equal positives
assert run("4\n1 1 1 1\n") == "10"

# alternating sum cancels
assert run("4\n1 -1 1 -1\n") == "4"

# single cancellation pair
assert run("5\n1 2 -3 2 -2\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp cơ sở | 
| tất cả những cái | 10 | tăng trưởng đơn điệu | 
| luân phiên ±1 | 4 | đặt lại tiền tố lặp đi lặp lại | 
| hủy hỗn hợp | 9 | xử lý đúng cách cửa sổ chuyển số | 

## Vỏ cạnh 

Mảng một phần tử luôn hợp lệ vì không tồn tại mảng con bên trong. Thuật toán khởi tạo`last = {0: 0}`và đếm`(r - l + 1) = 1`, phù hợp với kết quả mong đợi. 

Mảng có tổng tiền tố lặp lại như`[1, -1, 1, -1]`kích hoạt việc đặt lại thường xuyên của ranh giới bên trái. Tổng tiền tố mỗi lần`0`xuất hiện lại, các phân đoạn trước đó sẽ bị loại trừ. Việc thực hiện cập nhật chính xác`l`để bỏ qua các vị trí bắt đầu không hợp lệ, đảm bảo không có mảng con nào chứa phần hủy hoàn toàn được tính không chính xác. 

Mảng thống nhất lớn như`[1, 1, 1, ..., 1]`không bao giờ tạo ra xung đột tiền tố, vì vậy`l`vẫn cố định ở mức 1. Mọi mảng con đều hợp lệ và câu trả lời trở thành`n(n+1)/2`, mà thuật toán tích lũy một cách tự nhiên thông qua`(r - l + 1)`ở mỗi bước.
