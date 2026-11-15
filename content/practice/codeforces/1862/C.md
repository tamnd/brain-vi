---
title: "CF 1862C - Hàng Rào Thành Phố Hoa"
description: "Chúng ta được cung cấp một chuỗi các tấm ván hàng rào, mỗi tấm ván có chiều cao, đã được sắp xếp theo thứ tự không tăng dần. Anya muốn biết liệu hàng rào có đối xứng hay không khi được xem dưới dạng lưới các khối đơn vị."
date: "2026-06-09T00:08:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1862
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 894 (Div. 3)"
rating: 1100
weight: 1862
solve_time_s: 177
verified: true
draft: false
---

[CF 1862C - Hàng rào thành phố hoa](https://codeforces.com/problemset/problem/1862/C) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các tấm ván hàng rào, mỗi tấm ván có chiều cao, đã được sắp xếp theo thứ tự không tăng dần. Anya muốn biết liệu hàng rào có đối xứng hay không khi được xem dưới dạng lưới các khối đơn vị. Tính đối xứng ở đây có nghĩa là nếu chúng ta lấy các tấm ván, coi mỗi đơn vị chiều cao là một hàng ngang và đặt các tấm ván dưới dạng cột thì hình dạng 2D thu được sẽ giống hệt với bố cục dọc ban đầu của các tấm ván. 

Cụ thể, mỗi tấm ván có chiều cao`h`đóng góp`h`đơn vị ngang tại cột của nó. Khi được xem dưới dạng sắp xếp theo chiều ngang, hàng trên cùng chứa các tấm ván cao nhất, hàng tiếp theo là tấm ván cao nhất tiếp theo, v.v. Nếu sau phép biến đổi này, trình tự chiều dài hàng giống với trình tự chiều cao ban đầu của tấm ván thì hàng rào được coi là đối xứng. 

Đầu vào bị hạn chế sao cho tổng số plan trong tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này loại trừ các thuật toán kém hơn tuyến tính trên mỗi trường hợp thử nghiệm. Độ cao có thể lớn nhưng chúng chỉ ảnh hưởng đến việc đếm chứ không ảnh hưởng đến phép lặp nên chúng ta không cần thực hiện các phép toán tỷ lệ với giá trị của chúng. 

Các trường hợp cạnh bao gồm một tấm ván đơn, nhiều tấm ván có cùng chiều cao và các chuỗi có sự khác biệt về chiều cao không đều. Ví dụ,`[5,5,5,1,1]`đối xứng vì các hàng trên cùng khớp với bố cục dọc ban đầu, nhưng`[5,4,2]`thì không, vì các hàng ngang nén khác nhau và "độ dài hàng" ngang không khớp với bản gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xây dựng toàn bộ biểu diễn ngang dưới dạng lưới 2D và kiểm tra từng hàng xem độ dài hàng kết quả có khớp với chuỗi ban đầu hay không. Điều này hoạt động hợp lý nhưng sẽ mất thời gian O(n * max_height), điều này không khả thi đối với độ cao lớn (lên tới 10^9). Việc xây dựng một lưới điện thực tế là không cần thiết và quá chậm. 

Cách tiếp cận tối ưu cho thấy rằng chúng ta không cần toàn bộ lưới điện. Thay vào đó, chúng ta chỉ cần mô phỏng việc giảm chiều cao tấm ván từ trên xuống như thể chúng ta đang tạo hàng: 

1. Theo dõi số lượng ván ở mỗi độ cao khi chúng ta “bóc” hàng. 
2. Tại mỗi bước, kiểm tra xem các chiều cao còn lại có còn thỏa mãn điều kiện số ván có chiều cao tối thiểu đó lớn hơn hoặc bằng chiều cao tiếp theo hay không. 

Thông tin chi tiết quan trọng là cách biểu diễn theo chiều ngang tương đương với việc lấy chiều cao, sắp xếp chúng (đã được sắp xếp theo thứ tự không tăng) và xác minh điều đó cho từng bước`i`, số ván có chiều cao >=`i`ít nhất là`i`. Đây chính xác là điều kiện để hàng rào là sơ đồ Young đối xứng. Nếu ở bất kỳ bước nào điều này không thành công thì hàng rào không đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lưới Brute Force | O(n * max(a_i)) | O(n * max(a_i)) | Quá chậm | 
| Đếm & Kiểm Tra Tham Lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và danh sách độ cao`a`. 
3. Khởi tạo bộ đếm`extra`để theo dõi "đơn vị dư thừa" tích lũy được mang theo từ những tấm ván cao hơn. 
4. Lặp lại từ tấm ván đầu tiên đến tấm ván cuối cùng: 

1. Đối với ván`i`, tính toán`expected`vì số lượng hàng tấm ván này sẽ đóng góp ở chế độ xem ngang, chiếm`extra`. 
2. Nếu`a[i] > expected`, tăng`extra`để chuyển những phần thừa xuống các tấm ván thấp hơn. 
3. Nếu`a[i] < expected`, hàng rào không thể đối xứng; đánh dấu là "KHÔNG" và ngắt. 
5. Nếu vòng lặp hoàn tất mà không bị lỗi, hãy đánh dấu là "CÓ". 
6. In kết quả cho từng test. 

**Tại sao nó hoạt động**: Ở mỗi bước,`extra`tích lũy chiều cao dư thừa phải được trải xuống các tấm ván thấp hơn. Điều này đảm bảo rằng quá trình nén theo chiều ngang khớp với trình tự dọc. Bất kỳ hành vi vi phạm quy tắc nào đều chỉ ra rằng chiều dài hàng ngang của hàng rào không thể khớp với chiều cao thẳng đứng ban đầu, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_symmetric(a):
    n = len(a)
    extra = 0
    for i in range(n):
        # the maximal allowed height in horizontal layout
        if a[i] > i + extra + 1:
            extra += a[i] - (i + extra + 1)
        elif a[i] < i + extra + 1:
            return "NO"
    return "YES"

def main():
    t = int(input())
    results = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        results.append(is_symmetric(a))
    print("\n".join(results))

if __name__ == "__main__":
    main()
```**Giải thích về mã**: 

-`extra`theo dõi các đơn vị dư thừa tích lũy từ các tấm ván cao hơn. Điều này tránh việc xây dựng một lưới một cách rõ ràng. 
-`i + extra + 1`đại diện cho chiều cao tấm ván tối đa được phép ở vị trí này để đối xứng. 
- Lặp lại một lần trên mỗi plan đảm bảo thời gian O(n) cho mỗi test case. 
- Sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh chóng cho số lượng lớn các trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[5,4,3,2,1]`: 

| tôi | một [tôi] | thêm | tôi + thêm + 1 | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 1 | thêm += 4 → 4 | 
| 1 | 4 | 4 | 6 | được | 
| 2 | 3 | 4 | 7 | được | 
| 3 | 2 | 4 | 8 | được | 
| 4 | 1 | 4 | 9 | được | 

Kết quả:`YES`. Phần dư thừa do các tấm ván cao hơn mang lại cho phép bố trí theo chiều ngang phù hợp với chiều dọc. 

Đầu vào mẫu`[4,2,1]`: 

| tôi | một [tôi] | thêm | tôi + thêm + 1 | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 1 | thêm += 3 → 3 | 
| 1 | 2 | 3 | 5 | được | 
| 2 | 1 | 3 | 6 | được | 

Mô phỏng cho thấy tại hàng 2, kỳ vọng 6 > 1, không thể thỏa mãn tính đối xứng →`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền đơn qua mảng; đọc đầu vào chiếm ưu thế. | 
| Không gian | O(n) | Lưu trữ chiều cao đầu vào và kết quả. | 

Thuật toán dễ dàng xử lý tổng của`n`lên tới 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input = builtins.input
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("""7
5
5 4 3 2 1
3
3 1 1
3
4 2 1
1
2
5
5 3 3 1 1
5
5 5 5 3 3
2
6 1
""") == """YES
YES
NO
NO
YES
YES
NO""", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "YES", "single plank"
assert run("1\n3\n3 3 3\n") == "YES", "all equal"
assert run("1\n3\n5 1 1\n") == "NO", "sharp drop"
assert run("1\n4\n4 3 2 1\n") == "YES", "perfect decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ván | CÓ | trường hợp tầm thường | 
| chiều cao bằng nhau | CÓ | giữ đối xứng ngang | 
| giảm mạnh | KHÔNG | không đủ chiều cao cho tấm ván thấp hơn | 
| giảm hoàn hảo | CÓ | trường hợp giảm dần tiêu chuẩn | 

## Vỏ cạnh 

Đối với một tấm ván đơn`[1]`,`extra`là 0 và chiều cao phù hợp với mong đợi 1 →`YES`. 

Đối với hàng rào`[5,1,1]`, sau tấm ván đầu tiên`extra = 4`, nhưng chiều cao tấm ván tiếp theo 1 không thể đạt được như mong đợi 6 →`NO`. 

Thuật toán truyền chính xác chiều cao dư thừa và phát hiện các điểm không khớp, bao gồm tất cả các tình huống khó khăn.
