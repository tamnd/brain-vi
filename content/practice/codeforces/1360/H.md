---
problem: 1360H
contest_id: 1360
problem_index: H
name: "Binary Median"
contest_name: "Codeforces Round 644 (Div. 3)"
rating: 2100
tags: ["binary search", "bitmasks", "brute force", "constructive algorithms"]
answer: passed_samples
verified: true
solve_time_s: 277
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e3ffc-019c-83ec-9f2e-0b9dbb143a88
---

# CF 1360H - Trung vị nhị phân 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu, thuật toán xây dựng 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e3ffc-019c-83ec-9f2e-0b9dbb143a88 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta bắt đầu với không gian đầy đủ của tất cả các chuỗi nhị phân có độ dài`m`, được sắp xếp theo thứ tự từ điển tiêu chuẩn, tương đương với việc sắp xếp chúng dưới dạng số nhị phân có độ dài cố định từ`0...0`lên đến`1...1`. Từ bộ thứ tự hoàn chỉnh này, một bộ sưu tập nhỏ lên tới 100 chuỗi sẽ bị xóa. Sau những lần loại bỏ này, về mặt khái niệm, chúng tôi xem xét các chuỗi còn lại, sắp xếp lại chúng theo thứ tự từ điển và chọn phần tử ở giữa, trong đó “giữa” được xác định là phần tử tại chỉ mục ⌊(k−1)/2⌋ trong số k chuỗi còn lại. 

Một cách hữu ích để suy nghĩ về cấu trúc là tập hợp đầy đủ là khoảng`[0, 2^m − 1]`, được hiểu là số nhị phân có số 0 đứng đầu. Việc xóa chuỗi sẽ xóa tới 100 điểm khỏi vũ trụ có trật tự rất lớn này. Nhiệm vụ là xác định giá trị trung bình của những gì còn lại mà không cần xây dựng hoặc sắp xếp toàn bộ. 

Các ràng buộc là tín hiệu quan trọng. chiều dài`m`có thể lớn tới 60, điều này làm cho`2^m`lớn về mặt thiên văn, vì vậy bất kỳ phương pháp nào lặp qua tất cả các chuỗi đều không thể thực hiện được. Tuy nhiên, số lượng phần tử bị loại bỏ rất nhỏ, tối đa là 100 phần tử cho mỗi trường hợp thử nghiệm và tổng kích thước đầu vào trong các thử nghiệm là bị giới hạn. Sự bất đối xứng này gợi ý rõ ràng rằng câu trả lời phụ thuộc vào cách loại bỏ các vị trí thay đổi theo trật tự toàn cầu thay vì vào các trạng thái liệt kê. 

Một cách tiếp cận ngây thơ sẽ là tạo ra tất cả`2^m`chuỗi, xóa những chuỗi bị cấm, sắp xếp và chọn số trung vị. Điều này ngay lập tức thất bại vì ngay cả đối với`m = 60`, kích thước vũ trụ vượt xa mọi giới hạn tính toán. 

Một cạm bẫy tinh vi khác là cố gắng tìm kiếm nhị phân câu trả lời bằng cách kiểm tra thứ hạng thông qua việc đếm xem có bao nhiêu chuỗi hợp lệ nhỏ hơn một ứng cử viên. Nếu được triển khai một cách ngây thơ, điều này vẫn yêu cầu lặp lại tất cả`2^m`khả năng hoặc mô phỏng số lượng lớn không hiệu quả. Cách tiếp cận đúng phải tránh chạm vào toàn bộ không gian. 

## Phương pháp tiếp cận 

Quan điểm brute-force rất đơn giản: liệt kê tất cả các chuỗi nhị phân có độ dài`m`, xóa những cái bị cấm, sắp xếp danh sách còn lại và chọn cái ở giữa. Điều này hoạt động về mặt khái niệm vì thứ tự từ điển khớp với thứ tự số trên biểu diễn nhị phân. Tuy nhiên, chi phí có tính cấp số nhân: tạo ra`2^60`các ứng cử viên là không thể, và thậm chí việc lưu trữ chúng cũng không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần bộ đầy đủ. Chúng ta chỉ cần biết có bao nhiêu chuỗi hợp lệ nằm trước vị trí ứng viên. Vì chỉ có tối đa 100 chuỗi bị loại bỏ nên cấu trúc của phần bù gần như hoàn chỉnh. Mỗi chuỗi bị loại bỏ chỉ tạo ra một “khoảng trống” nhỏ theo thứ tự liên tục. 

Điều này gợi ý suy nghĩ về mặt thống kê thứ tự trong một khoảng thời gian gần như đầy đủ. Thay vì lặp lại tất cả các ứng viên, chúng ta có thể tìm kiếm nhị phân câu trả lời trong phạm vi`[0, 2^m - 1]`. Đối với số ứng viên`x`, chúng tôi tính toán có bao nhiêu chuỗi hợp lệ ≤ x. Điều này đơn giản là`(x + 1) - count_removed_leq(x)`. Từ`n ≤ 100`, việc đếm các phần tử bị loại bỏ ≤ x là O(n), giúp mỗi lần kiểm tra được nhanh chóng. 

Tìm kiếm nhị phân trên không gian 60 bit yêu cầu tối đa 60 lần lặp và mỗi lần lặp có giá O(n), vì vậy tổng số lần lặp là rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m log 2^m) | O(2^m) | Không thể | 
| Tối ưu | O(n log 2^m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi chuỗi nhị phân là số nguyên 60 bit. 

1. Chuyển đổi tất cả các chuỗi bị loại bỏ thành số nguyên. Điều này cho phép so sánh trực tiếp bằng cách sử dụng thứ tự số, khớp với thứ tự từ điển do độ dài cố định. Bước này rất cần thiết vì nó biến so sánh chuỗi thành so sánh số học. 
2. Sắp xếp các giá trị bị loại bỏ. Sắp xếp cho phép đếm tiền tố nhanh bằng cách sử dụng tìm kiếm nhị phân. 
3. Xác định hàm`removed_leq(x)`trả về số lượng số bị loại bỏ ≤ x bằng cách sử dụng tìm kiếm nhị phân trên mảng đã loại bỏ được sắp xếp. Điều này cho phép chúng tôi truy cập nhanh vào “có bao nhiêu giá trị bị cấm trong tiền tố lên đến x”. 
4. Xác định`valid_leq(x) = (x + 1) - removed_leq(x)`. Điều này thể hiện có bao nhiêu chuỗi hợp lệ nằm trong`[0, x]`. 
5. Tính thứ hạng mục tiêu`target = (2^m - n - 1) // 2`. Đây là chỉ số trung bình trong lập chỉ mục dựa trên số 0. 
6. Tìm kiếm nhị phân nhỏ nhất`x`như vậy`valid_leq(x) > target`. Cái đó`x`chính xác là chuỗi trung vị. 

Tìm kiếm nhị phân hoạt động trên phạm vi số đầy đủ`[0, 2^m - 1]`, điều này an toàn vì tất cả các chuỗi hợp lệ đều nằm bên trong nó. 

### Tại sao nó hoạt động 

chức năng`valid_leq(x)`là đơn điệu trong`x`, vì ngày càng tăng`x`chỉ có thể tăng số lượng chuỗi hợp lệ trong tiền tố. Điều này đảm bảo rằng tìm kiếm nhị phân là hợp lệ. Mỗi chuỗi bị loại bỏ sẽ trừ chính xác một điểm khỏi số tiền tố, do đó độ lệch so với tiền tố đầy đủ`[0, x]`luôn chính xác là số lượng phần tử bị cấm bên trong nó. Kết quả là chúng tôi khôi phục thông tin xếp hạng chính xác mà không cần liệt kê tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_int(s):
    return int(s, 2)

def count_leq(arr, x):
    # number of elements <= x in sorted arr
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        rem = [to_int(input().strip()) for _ in range(n)]
        rem.sort()

        total = (1 << m)
        k = total - n
        target = (k - 1) // 2

        def valid_leq(x):
            return (x + 1) - count_leq(rem, x)

        lo, hi = 0, (1 << m) - 1
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if valid_leq(mid) > target:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        s = bin(ans)[2:].zfill(m)
        print(s)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc xử lý các chuỗi nhị phân dưới dạng số nguyên, loại bỏ mọi sự phức tạp về từ điển. Danh sách loại bỏ được sắp xếp chỉ được sử dụng để đếm các loại trừ một cách hiệu quả. Tìm kiếm nhị phân tìm vị trí xếp hạng chính xác mà không cần xây dựng toàn bộ. 

Một chi tiết triển khai tinh tế là đệm bằng`zfill(m)`, vì chuyển đổi số giảm các số 0 đứng đầu nhưng đầu ra phải duy trì độ dài cố định. Một điểm quan trọng khác là sử dụng`x + 1`TRONG`valid_leq(x)`, đảm bảo việc đếm chính xác trong phạm vi bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, m = 3
removed = [010, 001, 111]
```Đã sắp xếp các số nguyên bị loại bỏ:`[1, 2, 7]`Toàn dải là`[0..7]`, số đếm hợp lệ là`5`, chỉ số mục tiêu là`2`. 

| giữa (x) | hợp lệ_leq(x) | quyết định | 
| --- | --- | --- | 
| 3 | 2 | đi bên phải | 
| 5 | 4 | đi bên trái | 
| 4 | 3 | đi bên trái | 

Câu trả lời cuối cùng là`4`→`100`. 

Dấu vết này cho thấy cách đếm tiền tố tránh liệt kê tập hợp hợp lệ một cách rõ ràng. 

### Ví dụ 2 

đầu vào:```
n = 1, m = 1
removed = [1]
```Đặt còn lại là`[0]`, vậy đáp án là`0`. 

Tìm kiếm nhị phân hoạt động trên`[0,1]`: 

| giữa | hợp lệ_leq(giữa) | quyết định | 
| --- | --- | --- | 
| 0 | 1 | tìm thấy | 

Điều này chứng tỏ tính đúng đắn khi vũ trụ sụp đổ thành một phần tử hợp lệ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log 2^m) | tìm kiếm nhị phân trên phạm vi 60 bit, mỗi bước sẽ tính các phần tử bị loại bỏ | 
| Không gian | O(n) | lưu trữ các giá trị đã xóa | 

Tìm kiếm nhị phân chạy tối đa 60 lần lặp và mỗi lần lặp thực hiện tìm kiếm nhị phân trên tối đa 100 phần tử. Điều này dễ dàng phù hợp với giới hạn ngay cả đối với 1000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""5
3 3
010
001
111
4 3
000
111
100
011
1 1
1
1 1
0
3 2
00
01
10
""") == """100
010
0
1
11"""

# single element left
assert run("""1
1 1
1
""") == "0"

# no removals except one
assert run("""1
1 2
11
""") == "10"

# maximum m small n
assert run("""1
2 3
000
111
""") == "011"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | hỗn hợp | tính đúng đắn trong trường hợp tổng quát | 
| 1 1 đã xóa | 0 | cạnh nhỏ nhất | 
| m=2 loại bỏ duy nhất | 10 | dịch chuyển tiền tố | 
| loại bỏ thưa thớt | 011 | ổn định xếp hạng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi số trung vị nằm ngay cạnh một số bị loại bỏ. Ví dụ, nếu`m = 3`và chúng tôi loại bỏ`011`, phạm vi được phân chia xung quanh một điểm bị thiếu. Thuật toán vẫn hoạt động vì việc đếm tiền tố tự nhiên bỏ qua giá trị còn thiếu mà không cần xử lý khoảng cách rõ ràng. 

Một trường hợp khác là khi cụm giá trị bị loại bỏ ở đầu phạm vi. Giả sử chúng ta loại bỏ`000`,`001`,`010`. Chức năng tiền tố`valid_leq(x)`nén chính xác phân đoạn đầu để tìm kiếm nhị phân bắt đầu hiệu quả từ khối hợp lệ đầu tiên`011`. Mặc dù khoảng cách số lớn, việc điều chỉnh số lượng vẫn tuyến tính theo số lần loại bỏ, do đó tính chính xác vẫn được duy trì. 

Cuối cùng, khi`n = 0`, câu trả lời chỉ đơn giản là phần giữa của toàn bộ phạm vi mà tìm kiếm nhị phân cũng tìm thấy vì`removed_leq(x)`luôn bằng 0 và cấu trúc giảm về mức đếm thuần túy`[0, 2^m - 1]`.
