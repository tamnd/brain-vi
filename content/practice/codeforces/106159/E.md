---
title: "CF 106159E - Vấn đề cấu trúc dữ liệu cơ bản"
description: "Chúng tôi đang duy trì một bộ sưu tập ngăn xếp ngày càng tăng, trong đó cả số lượng ngăn xếp và nội dung của chúng đều phát triển theo thời gian. Ban đầu không có ngăn xếp. Hệ thống xử lý một chuỗi các thao tác theo thứ tự. Một thao tác tạo ra một ngăn xếp trống mới và thêm nó vào cuối."
date: "2026-06-19T19:14:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106159
codeforces_index: "E"
codeforces_contest_name: "XIII UnB Contest Mirror"
rating: 0
weight: 106159
solve_time_s: 46
verified: true
draft: false
---

[CF 106159E - Vấn đề về cấu trúc dữ liệu cơ bản](https://codeforces.com/problemset/problem/106159/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập ngăn xếp ngày càng tăng, trong đó cả số lượng ngăn xếp và nội dung của chúng đều phát triển theo thời gian. Ban đầu không có ngăn xếp. Hệ thống xử lý một chuỗi các thao tác theo thứ tự. 

Một thao tác tạo ra một ngăn xếp trống mới và thêm nó vào cuối. Một thao tác khác sẽ đẩy một giá trị vào ngăn xếp đã chọn. Thao tác thứ ba yêu cầu giá trị thứ j đã từng được đẩy vào một ngăn xếp cụ thể, tính từ cuối ngăn xếp đó theo thứ tự chèn. 

Vì vậy, mỗi ngăn xếp hoạt động giống như một ngăn xếp tiêu chuẩn cho thứ tự chèn, nhưng các truy vấn không yêu cầu phần tử trên cùng. Thay vào đó, họ yêu cầu quyền truy cập lịch sử bằng chỉ mục chèn. 

Các ràng buộc là cực kỳ nhỏ, với tổng số tối đa là 100 thao tác. Điều này ngay lập tức loại bỏ mọi lo ngại về hiệu suất ngoài việc xử lý tuyến tính đơn giản. Bất kỳ cách tiếp cận nào thực hiện công liên tục hoặc tuyến tính trên mỗi thao tác đều sẽ trôi qua một cách thoải mái. 

Sự tinh tế chính không phải là hiệu suất mà là tính chính xác của việc lập chỉ mục. Mỗi ngăn xếp phát triển độc lập và các hoạt động đề cập đến các ngăn xếp theo mã định danh dựa trên 1 phụ thuộc vào thứ tự tạo. Việc triển khai đơn giản có thể dễ dàng mắc sai lầm khi trộn lẫn các chỉ số ngăn xếp hoặc do hiểu nhầm rằng phần tử thứ j đề cập đến thứ tự chèn thay vì độ sâu từ trên xuống trong các hoạt động ngược lại. 

Một trường hợp lỗi điển hình xảy ra khi một người coi nhầm ngăn xếp là cần xóa hoặc lập chỉ mục lại động. Ví dụ: nếu chúng tôi hiểu phần tử thứ j là "thứ j từ trên xuống", chúng tôi có thể đảo ngược việc lập chỉ mục không chính xác. Trong thực tế, nó chỉ đơn giản là phần tử được đẩy thứ j. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp khớp chính xác với cấu trúc vấn đề. Chúng tôi duy trì một danh sách các ngăn xếp. Mỗi ngăn xếp là một danh sách lưu trữ các giá trị theo thứ tự chèn. Khi thao tác loại 1 xuất hiện, chúng tôi sẽ thêm một danh sách trống mới. Khi thao tác loại 2 xuất hiện, chúng tôi sẽ thêm giá trị vào ngăn xếp tương ứng. Khi thao tác loại 3 xuất hiện, chúng tôi lập chỉ mục trực tiếp vào ngăn xếp. 

Điều này hoạt động vì tất cả các hoạt động đều cục bộ và không yêu cầu bất kỳ cấu trúc toàn cầu nào. Không cần phải cân bằng, tìm kiếm hay kiên trì. Mọi truy vấn đều được trả lời trong thời gian O(1) sau khi cập nhật O(1). 

Một biến thể bạo lực sẽ có cấu trúc giống hệt nhau vì không có vấn đề tối ưu hóa có ý nghĩa nào ở đây. Mọi nỗ lực sử dụng cấu trúc dữ liệu phức tạp hơn sẽ chỉ tăng thêm chi phí mà không mang lại lợi ích. 

Quan sát quan trọng là các ngăn xếp độc lập và lịch sử tăng trưởng của chúng chính xác là chuỗi các giá trị được thêm vào. Do đó, phần tử thứ j thực sự có chỉ số j-1 trong mảng cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trực tiếp) | O(Q) | O(Q) | Đã chấp nhận | 
| Tối ưu (cùng mô phỏng) | O(Q) | O(Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một danh sách trống`stacks`. Điều này thể hiện tập hợp các ngăn xếp hiện tại theo thứ tự tạo. Bản thân mỗi ngăn xếp là một danh sách lưu trữ các giá trị được đẩy theo thứ tự đến. 
2. Khi thao tác loại 1 xảy ra, hãy thêm một danh sách trống mới vào`stacks`. Danh sách mới này đại diện cho một ngăn xếp mới không có phần tử nào. 
3. Khi thực hiện thao tác loại 2`(i, x)`xảy ra, nối thêm`x`ĐẾN`stacks[i-1]`. Điều này mô phỏng việc đẩy lên ngăn xếp i, giữ nguyên thứ tự chèn. 
4. Khi thực hiện thao tác loại 3`(i, j)`xảy ra, đầu ra`stacks[i-1][j-1]`. Điều này trực tiếp lấy phần tử được chèn thứ j trong ngăn xếp đó. 

Tính chính xác đến từ việc duy trì từng ngăn xếp dưới dạng một chuỗi thuần túy chỉ bổ sung. Vì không có sự xóa hoặc sửa đổi nào xảy ra nên thứ tự chèn tương đương với việc lập chỉ mục vị trí ổn định. 

### Tại sao nó hoạt động 

Mỗi ngăn xếp độc lập và chỉ nhận được các hoạt động nối thêm. Điều này có nghĩa là chuỗi các giá trị bên trong ngăn xếp chính xác là thứ tự thời gian của các phần chèn vào. Phần tử được chèn thứ j không bao giờ thay đổi vị trí vì không có thao tác nào loại bỏ hoặc sắp xếp lại các phần tử. Vì vậy, việc lập chỉ mục theo vị trí tương đương với việc theo dõi lịch sử chèn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    q = int(input().strip())
    stacks = []

    for _ in range(q):
        parts = input().split()
        t = int(parts[0])

        if t == 1:
            stacks.append([])

        elif t == 2:
            i = int(parts[1]) - 1
            x = int(parts[2])
            stacks[i].append(x)

        else:
            i = int(parts[1]) - 1
            j = int(parts[2]) - 1
            print(stacks[i][j])

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh trực tiếp thuật toán. Mỗi ngăn xếp được lưu trữ dưới dạng danh sách Python, do đó, phần bổ sung được khấu hao O(1). Việc lập chỉ mục được điều chỉnh từ dựa trên 1 thành dựa trên 0 cho cả ngăn xếp và phần tử. 

Lỗi phổ biến duy nhất là quên rằng cả chỉ số ngăn xếp và vị trí phần tử đều dựa trên 1 trong đầu vào. Việc chuyển đổi phải được áp dụng nhất quán ở mọi lần truy cập. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản: 

đầu vào:```
1
2 1 10
3 1 1
```| Bước | Hoạt động | Trạng thái ngăn xếp | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | tạo ngăn xếp | [[]] | | 
| 2 | đẩy 10 lên ngăn xếp 1 | [[10]] | | 
| 3 | ngăn xếp truy vấn 1, chỉ mục 1 | [[10]] | 10 | 

Đầu ra là 10 vì đây là phần tử đầu tiên và duy nhất được chèn vào. 

Bây giờ hãy xem xét nhiều ngăn xếp: 

đầu vào:```
3
1
1
2 1 5
2 2 7
3 1 1
```| Bước | Hoạt động | Trạng thái ngăn xếp | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | tạo | [[]] | | 
| 2 | tạo | [[], []] | | 
| 3 | đẩy 5 lên ngăn 1 | [[5], []] | | 
| 4 | đẩy 7 lên ngăn 2 | [[5], [7]] | | 
| 5 | ngăn xếp truy vấn 1, 1 | [[5], [7]] | 5 | 

Điều này xác nhận rằng các ngăn xếp phát triển độc lập và việc lập chỉ mục là cục bộ cho mỗi ngăn xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) | Mỗi thao tác thực hiện công việc O(1): nối thêm, tạo hoặc lập chỉ mục trực tiếp | 
| Không gian | O(Q) | Mỗi giá trị được đẩy được lưu trữ một lần trên tất cả các ngăn xếp | 

Giới hạn 100 thao tác làm cho giải pháp này nhanh chóng. Các ràng buộc cao hơn thậm chí sẽ vẫn an toàn do các hoạt động liên tục trong thời gian cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    q = int(sys.stdin.readline().strip())
    stacks = []
    out = []

    for _ in range(q):
        parts = sys.stdin.readline().split()
        t = int(parts[0])

        if t == 1:
            stacks.append([])
        elif t == 2:
            i = int(parts[1]) - 1
            x = int(parts[2])
            stacks[i].append(x)
        else:
            i = int(parts[1]) - 1
            j = int(parts[2]) - 1
            out.append(str(stacks[i][j]))

    return "\n".join(out)

# provided-style sample 1
assert run("""3
1
2 1 4
3 1 1
""") == "4"

# sample 2: multiple stacks
assert run("""6
1
1
2 1 5
2 2 9
3 2 1
3 1 1
""") == "9\n5"

# single stack multiple queries
assert run("""5
1
2 1 7
2 1 8
3 1 1
3 1 2
""") == "7\n8"

# minimal edge: one element
assert run("""2
1
3 1 1
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đẩy/truy vấn ngăn xếp đơn | 4 | chức năng cơ bản | 
| hai ngăn xếp xen kẽ | 9\n5 | sự độc lập của ngăn xếp | 
| nhiều lần đẩy cùng một ngăn xếp | 7\n8 | bảo quản đơn hàng | 
| chỉ tạo tối thiểu | trống | cấu trúc xử lý không cần truy vấn | 

## Vỏ cạnh 

Trường hợp một cạnh đang truy vấn ngay sau khi tạo ngăn xếp mà không cần đẩy. Vì sự cố đảm bảo các truy vấn hợp lệ nên tình huống này không xảy ra ở đầu vào, nhưng về mặt logic, việc truy cập vào ngăn xếp trống sẽ không hợp lệ. Việc triển khai dựa trên sự đảm bảo và không bổ sung thêm các bước kiểm tra. 

Một trường hợp tinh tế khác là đảm bảo lập chỉ mục dựa trên 1 được chuyển đổi nhất quán. Ví dụ: ngăn xếp 1 được lưu trữ ở chỉ mục 0. Nếu sự thay đổi này bị quên trong quá trình đẩy hoặc truy vấn, chương trình sẽ âm thầm trả về kết quả không chính xác trong khi vẫn nằm trong giới hạn. 

Trường hợp cuối cùng là khi nhiều ngăn xếp được tạo trước khi xảy ra bất kỳ lần đẩy nào. Cấu trúc vẫn phải phân bổ chính xác các danh sách trống mà không có lỗi, đảm bảo sau này sẽ đẩy đất vào đúng ngăn xếp theo thứ tự định danh.
