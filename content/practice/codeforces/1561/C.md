---
title: "CF 1561C - Sâu bên dưới"
description: "Chúng ta được giao cho một anh hùng đối mặt với một dãy hang động. Mỗi hang động chứa một số quái vật, mỗi quái vật có một giá trị áo giáp và người anh hùng chỉ có thể đánh bại một con quái vật nếu sức mạnh hiện tại của anh ta thực sự lớn hơn áo giáp của quái vật."
date: "2026-06-10T12:13:29+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1561
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 740 (Div. 2, based on VK Cup 2021 - Final (Engine))"
rating: 1300
weight: 1561
solve_time_s: 99
verified: true
draft: false
---

[CF 1561C - Sâu bên dưới](https://codeforces.com/problemset/problem/1561/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một anh hùng đối mặt với một dãy hang động. Mỗi hang động chứa một số quái vật, mỗi quái vật có một giá trị áo giáp và người anh hùng chỉ có thể đánh bại một con quái vật nếu sức mạnh hiện tại của anh ta thực sự lớn hơn áo giáp của quái vật. Sau khi đánh bại quái vật, sức mạnh của anh hùng sẽ tăng thêm một. Người anh hùng phải vào mỗi hang đúng một lần, chiến đấu với tất cả quái vật trong hang theo thứ tự nhất định và thoát ra thành công. Nhiệm vụ là xác định sức mạnh khởi đầu tối thiểu cho phép anh hùng dọn sạch tất cả các hang động theo một thứ tự nào đó. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng hang động và đối với mỗi hang động, số lượng quái vật và giá trị áo giáp của chúng theo thứ tự. Đầu ra là công suất khởi động tối thiểu cho mỗi trường hợp thử nghiệm. Các ràng buộc cho phép tổng cộng lên tới 100.000 hang động và 100.000 quái vật trong mỗi trường hợp thử nghiệm trên tất cả các trường hợp thử nghiệm. Điều này giới hạn tổng số thao tác ở mức khoảng 10^5 cho mỗi trường hợp thử nghiệm nếu chúng ta muốn một giải pháp chạy hiệu quả trong 2 giây. Điều này loại trừ bất kỳ thuật toán nào thử mọi hoán vị của thứ tự hang động, sẽ là giai thừa trong n. 

Một trường hợp không rõ ràng xảy ra khi hang động có quái vật có giá trị giáp giảm nghiêm trọng. Ví dụ, một hang động có quái vật`[10, 9, 8]`và cái khác với`[12]`yêu cầu ra lệnh cẩn thận: trước tiên anh hùng có thể cần phải giải quyết hang động bằng một con quái vật để giảm thiểu sức mạnh ban đầu. Một cách tiếp cận ngây thơ luôn chọn các hang động theo thứ tự đầu vào có thể đánh giá quá cao công suất ban đầu cần thiết. Tương tự, một hang động chỉ chứa một con quái vật có bộ giáp rất cao có thể chiếm ưu thế về sức mạnh ban đầu cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi hoán vị của trật tự hang động và mô phỏng tiến trình của anh hùng. Đối với mỗi mệnh lệnh, chúng tôi sẽ bắt đầu với một số sức mạnh, lặp lại qua từng hang động và đối với mỗi quái vật, hãy kiểm tra xem sức mạnh hiện tại của anh hùng có vượt quá áo giáp của quái vật hay không, tăng sức mạnh để chiến đấu thành công. Nếu anh hùng không thể đánh bại quái vật, chúng tôi sẽ loại bỏ sức mạnh ban đầu đó và thử giá trị cao hơn. Mặc dù cách tiếp cận này đúng về nguyên tắc, nhưng sự tăng trưởng giai thừa trong các hoán vị làm cho nó hoàn toàn không khả thi ngay cả đối với n vừa phải. 

Thông tin chi tiết quan trọng là trong mỗi hang động, sức mạnh ban đầu cần thiết tối thiểu của anh hùng phụ thuộc vào áo giáp của quái vật đầu tiên cộng với số lượng quái vật mà anh ta phải chiến đấu theo trình tự. Chính xác hơn là nếu quái vật trong hang động có áo giáp`[a_1, a_2, ..., a_k]`, sức mạnh ban đầu tối thiểu để sống sót trong hang động này là`max(a_j - (j - 1)) + 1`, Ở đâu`j`lập chỉ mục các quái vật theo thứ tự. Điều này giải thích cho việc tăng sức mạnh sau mỗi trận chiến. Khi chúng tôi tính toán yêu cầu này cho từng hang, vấn đề sẽ giảm xuống còn việc chọn thứ tự hang để giảm thiểu yêu cầu chạy tối đa. 

Chiến lược tối ưu là sắp xếp các hang động theo sức mạnh ban đầu cần thiết tối thiểu và mô phỏng người anh hùng đi qua chúng theo thứ tự đó. Chúng tôi theo dõi sức mạnh hiện tại, tăng sức mạnh theo số lượng quái vật trong hang và điều chỉnh linh hoạt sức mạnh khởi đầu cần thiết để đảm bảo anh hùng không bao giờ thất bại. Điều này tránh việc thử tất cả các hoán vị và đạt được độ phức tạp thời gian tuyến tính cho mỗi trường hợp thử nghiệm do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(K log n) | O(n) | Đã chấp nhận | 

Đây,`K`là tổng số quái vật trong tất cả các hang động. 

## Hướng dẫn thuật toán 

1. Đối với mỗi hang động, hãy tính công suất khởi động tối thiểu cần thiết để dọn sạch hang động đó. Lặp lại các quái vật theo thứ tự, duy trì sức mạnh cần thiết như`max(required, monster_armor - position_index + 1)`. Sau khi xử lý hang động, lưu trữ giá trị này cùng với số lượng quái vật trong hang động. 
2. Sắp xếp các hang động theo công suất khởi động yêu cầu tối thiểu được tính toán. Điều này ưu tiên các hang động dễ dọn dẹp hơn với sức mạnh anh hùng khởi đầu thấp hơn, giảm thiểu sức mạnh khởi đầu tối đa cần thiết trên tất cả các hang động. 
3. Khởi tạo`current_power`bằng 0 và`answer`bằng 0. Lặp lại các hang động được sắp xếp. Đối với mỗi hang động, cập nhật`answer`BẰNG`max(answer, minimal_required_power - current_power)`để đảm bảo anh hùng có thể vào hang. Sau đó tăng dần`current_power`bởi số lượng quái vật trong hang, phản ánh sức mạnh tích lũy được sau khi đánh bại tất cả quái vật trong hang đó. 
4. Sau khi xử lý hết các hang động,`answer`chứa sức mạnh ban đầu tối thiểu mà anh hùng phải bắt đầu để sống sót trong tất cả các hang động theo thứ tự tối ưu. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến ở đầu mỗi hang,`current_power + answer`ít nhất phải lớn bằng yêu cầu tối thiểu của hang động. Sắp xếp theo yêu cầu hang động tối thiểu đảm bảo mức tăng sức mạnh nhỏ nhất của anh hùng xảy ra trước tiên, điều này đảm bảo rằng sức mạnh ban đầu tổng thể được giảm thiểu. Bất kỳ thứ tự nào khác sẽ yêu cầu công suất khởi động cao hơn hoặc mang lại kết quả tương tự, xác nhận tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def minimal_start_power(caves):
    cave_info = []
    for monsters in caves:
        k = len(monsters)
        req = 0
        for i, armor in enumerate(monsters):
            req = max(req, armor - i + 1)
        cave_info.append((req, k))
    
    cave_info.sort()
    answer = 0
    current_power = 0
    for req, k in cave_info:
        answer = max(answer, req - current_power)
        current_power += k
    return answer

def main():
    t = int(input())
    results = []
    for _ in range(t):
        n = int(input())
        caves = []
        for _ in range(n):
            data = list(map(int, input().split()))
            k = data[0]
            monsters = data[1:]
            caves.append(monsters)
        results.append(str(minimal_start_power(caves)))
    print("\n".join(results))

if __name__ == "__main__":
    main()
```Giải pháp này tách quá trình tính toán của mỗi hang động thành một bước tiền xử lý để tính toán công suất khởi động cần thiết tối thiểu. Việc sắp xếp đảm bảo rằng các hang động được giải quyết theo thứ tự giảm thiểu sức mạnh ban đầu cần thiết của anh hùng. các`answer`biến chỉ được cập nhật khi công suất tích lũy hiện tại không đủ cho hang tiếp theo, ngăn chặn tình trạng quá tải. sử dụng`current_power += k`phản ánh mức tăng tích lũy sau mỗi hang và các số nguyên có kích thước tùy ý của Python tránh được các vấn đề tràn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
1
1
1 42
```| Hang | Quái vật | Công suất yêu cầu tối thiểu | Thứ tự sắp xếp | Nguồn điện hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [42] | 43 | 1 | 0 | 43 | 

Điều này xác nhận rằng một quái vật có áo giáp 42 yêu cầu sức mạnh ban đầu là 43. 

**Đầu vào mẫu 2:**```
1
2
3 10 15 8
2 12 11
```| Hang | Quái vật | Công suất yêu cầu tối thiểu | Thứ tự sắp xếp | Nguồn điện hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [3,10,15,8] | 15 | 2 | 0 | 13 | 
| 2 | [12,11] | 13 | 1 | 2 | 13 | 

Hang 2 được giải quyết đầu tiên với yêu cầu 13; sau khi đánh bại 2 con quái vật,`current_power=2`. Hang 1 yêu cầu 15, nhưng`current_power + answer = 2 + 13 = 15`, hợp lý. Người anh hùng sống sót với sức mạnh ban đầu tối thiểu 13. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K log n) | Tính toán trước công suất yêu cầu tối thiểu cho mỗi hang trong O(K), sắp xếp n hang trong O(n log n) | 
| Không gian | O(K + n) | Lưu trữ quái vật và sức mạnh tối thiểu trên mỗi hang động | 

Với K ≤ 10^5 và n ≤ 10^5, thuật toán phù hợp thoải mái trong vòng 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    output = io.StringIO()
    with redirect_stdout(output):
        main()
    return output.getvalue().strip()

# Provided samples
assert run("2\n1\n1 42\n2\n3 10 15 8\n2 12 11\n") == "43\n13", "samples"

# Minimum size
assert run("1\n1\n1 1\n") == "2", "single monster minimal"

# All equal
assert run("1\n2\n2 5 5\n2 5 5\n") == "6", "all equal monsters"

# Maximum monsters in one cave
assert run("1\n1\n5 1 2 3 4 5\n") == "2", "increasing sequence"

# Decreasing sequence
assert run("1\n1\n3 5 4 3\n") == "6", "decreasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 2 | Đầu vào nhỏ nhất | 
| 2 hang cả 5 | 6 | Quái vật bình đẳng | 
| 1 hang tăng | 2 | Anh hùng tăng dần đủ sức mạnh | 
| 1 hang giảm dần | 6 | Thuật toán xử lý giảm giáp chính xác | 

## Vỏ cạnh 

Một chuỗi quái vật giảm dần như`[5,4,3]`đòi hỏi tính toán cẩn thận. Công suất yêu cầu tối thiểu cho hang động là `max(5-0+1, 4-1+1, 3-2+1) = max(
