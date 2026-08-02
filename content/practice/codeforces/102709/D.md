---
title: "CF 102709D - Cụm thu phóng"
description: "Sự cố mô tả một hàng người trong cuộc gọi Zoom. Sau khi mọi người nhìn theo hướng đã chọn, mỗi người được biểu thị bằng L hoặc R, tùy thuộc vào hướng mà khuôn mặt của họ dường như chỉ vào trong ảnh chụp màn hình."
date: "2026-08-01T22:04:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102709
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 2"
rating: 0
weight: 102709
solve_time_s: 119
verified: true
draft: false
---

[CF 102709D - Cụm thu phóng](https://codeforces.com/problemset/problem/102709/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một hàng người trong cuộc gọi Zoom. Sau khi mọi người nhìn về hướng đã chọn, mỗi người được đại diện bởi một trong hai`L`hoặc`R`, tùy thuộc vào hướng mà khuôn mặt của họ dường như chỉ vào ảnh chụp màn hình. Một nhóm là một nhóm người liên tiếp tối đa, tất cả đều hướng về cùng một hướng. Nhiệm vụ là đếm xem có bao nhiêu nhóm như vậy xuất hiện ở dòng cuối cùng. 

Dữ liệu đầu vào cung cấp số lượng người được theo dõi theo chỉ dẫn của họ theo thứ tự từ trái sang phải. Đầu ra là số đoạn liền kề trong đó ký tự không đổi. 

Số người có thể đạt tới$10^5$, có nghĩa là nghiệm phải gần tuyến tính. Phương pháp kiểm tra từng cặp vị trí hoặc quét liên tục các phần lớn của đường dây sẽ hoạt động xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chỉ cần vượt qua vạch một lần là đủ vì chỉ cần so sánh hướng đi của mỗi người với người hàng xóm ngay gần họ. 

Các trường hợp khó khăn chính đến từ việc xử lý các thay đổi một cách chính xác và không tính sai từng người. Ví dụ:```
Input
1
L

Output
1
```Một người luôn tạo thành đúng một nhóm. Một giải pháp chỉ bắt đầu đếm sau khi tìm thấy sự thay đổi giữa các hàng xóm có thể vô tình trả về 0. 

Một trường hợp khác là:```
Input
5
L
L
L
L
L

Output
1
```Toàn bộ dòng là một cụm. Việc thực hiện bất cẩn có tính đến mọi lần xuất hiện của`L`sẽ trả lại năm thay vì một. 

Trường hợp cuối cùng là:```
Input
4
L
R
L
R

Output
4
```Mỗi cặp liền kề đều khác nhau nên mỗi người bắt đầu một cụm mới. Việc triển khai chỉ tính một loại hướng hoặc quên vị trí cuối cùng sẽ bỏ sót một số nhóm. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận vũ lực trực tiếp sẽ cố gắng xác định từng cụm bằng cách bắt đầu từ mỗi vị trí và mở rộng sang bên phải trong khi hướng vẫn giữ nguyên. Điều này đúng vì mọi phân đoạn cực đại đều có thể được tìm thấy bằng cách phát triển từ phần tử đầu tiên của nó. Tuy nhiên, trong trường hợp xấu nhất, chẳng hạn như một dòng chỉ chứa`L`nhân vật, chuyến thăm mở rộng đầu tiên$N$các yếu tố, chuyến thăm thứ hai$N-1$, vân vân. Tổng công việc trở thành:$$N + (N-1) + \dots + 1 = O(N^2)$$Vì$N = 100000$, đây là khoảng năm tỷ so sánh. 

Quan sát quan trọng là một cụm chỉ bắt đầu khi hướng thay đổi so với người trước đó. Chúng ta không cần biết toàn bộ chiều dài của một cụm. Chúng ta chỉ cần biết người hiện tại tiếp tục nhóm trước hay bắt đầu một nhóm mới. Điều này làm giảm vấn đề xuống còn một lần quét đơn giản trong đó mỗi cặp liền kề được kiểm tra một lần. 

Lực lượng vũ phu hoạt động vì nó phát hiện rõ ràng mọi phân đoạn, nhưng nó lặp lại cùng một thông tin nhiều lần. Việc quan sát ranh giới giữa các cụm chính xác là các vị trí mà các hướng lân cận khác nhau cho phép chúng ta đếm các ranh giới đó một cách trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình tự hướng dẫn. Người đầu tiên luôn bắt đầu cụm đầu tiên, vì vậy hãy khởi tạo câu trả lời cho một. 
2. Quét người từ vị trí thứ hai trở đi. So sánh hướng hiện tại với hướng trước đó. 
3. Nếu hai hướng khác nhau, hãy tăng số cụm vì một nhóm liền kề mới đã bắt đầu. 
4. Sau khi xử lý toàn bộ dòng, xuất số đếm. 

Lý do điều này có tác dụng là vì cách duy nhất để một nhóm mới xuất hiện là hai người lân cận quay mặt về những hướng khác nhau. Kích thước chính xác của cụm trước đó không thành vấn đề. 

Tại sao nó hoạt động: 

Trong quá trình quét, câu trả lời luôn bằng số cụm được hình thành bởi phần dòng đã được xử lý. Ban đầu, người đầu tiên tạo ra một cụm. Mỗi khi một hướng thay đổi, người hiện tại không thể thuộc về nhóm trước đó nên phải bắt đầu một nhóm mới. Khi quá trình quét kết thúc, mọi ranh giới có thể có đã được kiểm tra, do đó số lượng chính xác là số cụm trong toàn bộ dòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    directions = [input().strip() for _ in range(n)]

    ans = 1
    for i in range(1, n):
        if directions[i] != directions[i - 1]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu dưới dạng danh sách vì so sánh cần truy cập vào hướng trước đó. Vì chỉ có$10^5$ký tự, việc sử dụng bộ nhớ này là nhỏ. Thuật toán bắt đầu bằng một cụm vì một dòng không trống luôn có ít nhất một nhóm. 

Vòng lặp bắt đầu ở chỉ số một vì hướng đầu tiên không có hàng xóm trước đó để so sánh. Mỗi khi nhân vật hiện tại khác với nhân vật trước đó, người hiện tại sẽ trở thành thành viên đầu tiên của một nhóm mới. 

Không có phép tính ranh giới phức tạp hoặc mối quan tâm về kích thước số nguyên ở đây. Chi tiết triển khai quan trọng là tránh giả định đầu vào trống vì các ràng buộc đảm bảo có ít nhất một người. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
R
L
L
R
```quá trình quét hoạt động như sau: 

| Vị trí | Hướng | Hướng trước đó | Khối | 
| --- | --- | --- | --- | 
| 1 | R | không | 1 | 
| 2 | L | R | 2 | 
| 3 | L | L | 2 | 
| 4 | R | L | 3 | 

Sự thay đổi đầu tiên từ`R`ĐẾN`L`tạo thành cụm thứ hai. Giữa`L`tiếp tục cùng một nhóm. trận chung kết`R`bắt đầu một nhóm khác, đưa ra câu trả lời 3. 

Đối với đầu vào:```
5
L
L
R
R
L
```quá trình quét là: 

| Vị trí | Hướng | Hướng trước đó | Khối | 
| --- | --- | --- | --- | 
| 1 | L | không | 1 | 
| 2 | L | L | 1 | 
| 3 | R | L | 2 | 
| 4 | R | R | 2 | 
| 5 | L | R | 3 | 

Ví dụ này cho thấy các cụm dài được tính một lần. Hai liên tiếp`L`giá trị và hai liên tiếp`R`giá trị mỗi nhóm đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi người đều được khám một lần. | 
| Không gian | O(N) | Danh sách hướng lưu trữ chuỗi đầu vào. | 

Độ phức tạp về thời gian phù hợp với$10^5$hạn chế vì nó chỉ thực hiện một lượng công việc không đổi cho mỗi người. Việc sử dụng bộ nhớ cũng nằm trong giới hạn. Thuật toán có thể được giảm xuống còn O(1) không gian bổ sung bằng cách xử lý đầu vào trực tuyến, nhưng việc lưu trữ chuỗi giúp việc triển khai trở nên đơn giản. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""

    n = int(data[0])
    arr = data[1:]

    ans = 1
    for i in range(1, n):
        if arr[i] != arr[i - 1]:
            ans += 1

    return str(ans) + "\n"

def run(inp: str) -> str:
    return solve_io(inp)

assert run("""4
R
L
L
R
""") == "3\n", "sample 1"

assert run("""5
L
L
R
R
L
""") == "3\n", "sample 2"

assert run("""1
L
""") == "1\n", "single person"

assert run("""6
R
R
R
R
R
R
""") == "1\n", "all equal values"

assert run("""5
L
R
L
R
L
""") == "5\n", "alternating directions"

assert run("""7
L
L
L
R
R
L
L
""") == "3\n", "boundary changes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một người quay mặt sang trái | 1 | Xử lý kích thước tối thiểu | 
| Tất cả mọi người đều hướng về cùng một hướng | 1 | Không đếm sai các giá trị lặp lại | 
| Hướng thay thế | 5 | Mỗi thay đổi liền kề sẽ tạo ra một cụm | 
| Một số nhóm dài | 3 | Xử lý đúng ranh giới nhóm | 

## Vỏ cạnh 

Đối với một người độc thân:```
1
L
```thuật toán khởi tạo câu trả lời cho một và không bao giờ đi vào vòng lặp. Nó xuất ra một vì người duy nhất đã là một nhóm hoàn chỉnh. 

Đối với một dòng mà mọi người đều có cùng một hướng:```
5
L
L
L
L
L
```mọi so sánh đều tìm thấy những người hàng xóm bằng nhau, vì vậy câu trả lời không bao giờ tăng từ giá trị ban đầu của nó là một. Thuật toán xử lý chính xác toàn bộ dòng là một nhóm. 

Đối với đường dây xen kẽ hoàn toàn:```
4
L
R
L
R
```mọi so sánh đều phát hiện ra một sự thay đổi. Câu trả lời tăng dần từ một lên bốn, phù hợp với thực tế là mỗi người thuộc một nhóm riêng biệt.
