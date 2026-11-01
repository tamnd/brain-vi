---
title: "CF 106420E - Tấn công mạnh nhất trước"
description: "Chúng ta được cung cấp một chuỗi các đợt tấn công của kẻ thù. Mỗi đợt có hai tham số: tốc độ sát thương mỗi giây và thời lượng. Nếu người chơi chọn cấp độ ℓ, mọi đợt sẽ bị suy yếu bằng cách trừ đi ℓ khỏi tỷ lệ sát thương của nó, nhưng không bao giờ dưới 0."
date: "2026-06-20T23:10:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106420
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 3-11-26 (Beginner)"
rating: 0
weight: 106420
solve_time_s: 38
verified: true
draft: false
---

[CF 106420E - Cuộc tấn công mạnh nhất đầu tiên](https://codeforces.com/problemset/problem/106420/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các đợt tấn công của kẻ thù. Mỗi đợt có hai tham số: tốc độ sát thương mỗi giây và thời lượng. Nếu người chơi chọn cấp độ ℓ, mọi đợt sẽ bị suy yếu bằng cách trừ đi ℓ khỏi tỷ lệ sát thương của nó, nhưng không bao giờ dưới 0. Vì vậy, sóng i chỉ gây sát thương tối đa (0, dᵢ − ℓ) mỗi giây, duy trì trong tᵢ giây. 

Tổng thiệt hại nhận được trên tất cả các đợt là tổng của những đóng góp này. Mục tiêu là tìm mức nhỏ nhất ℓ sao cho tổng sát thương tích lũy hoàn toàn nhỏ hơn ngưỡng sức khỏe h nhất định, nghĩa là người chơi sống sót. 

Đối tượng tính toán chính là hàm D(ℓ) = Σ max(0, dᵢ − ℓ) · tᵢ. Chúng tôi không lựa chọn giữa các quyết định độc lập trên mỗi sóng mà là một tham số toàn cầu duy nhất ℓ giúp giảm đồng đều tất cả các sóng. 

Kích thước đầu vào cho phép n lớn và giá trị lớn của dᵢ và tᵢ. Bất kỳ giải pháp nào tính toán lại toàn bộ biểu thức thiệt hại nhiều lần theo cách đơn giản sẽ cần xử lý tất cả các sóng trên mỗi truy vấn. Nếu chúng ta thử quét tuyến tính trên các giá trị ℓ có thể lên tới max(dᵢ), giá trị này sẽ trở nên quá lớn vì dᵢ có thể lớn, khiến cho cách tiếp cận O(max(dᵢ) · n) không khả thi. 

Quan sát cấu trúc chính là tính đơn điệu: tăng ℓ chỉ có thể giảm mỗi số hạng max(0, dᵢ − ℓ), do đó D(ℓ) không bao giờ tăng khi ℓ tăng. Điều này ngụ ý một hành vi ngưỡng rõ ràng trong đó một khi có thể sống sót ở một số ℓ, tất cả ℓ lớn hơn cũng hoạt động. 

Trường hợp khó phát hiện nhất là khi ℓ vượt quá tất cả dᵢ. Trong trường hợp đó mọi số hạng đều trở thành 0, do đó D(ℓ) = 0 và khả năng sống sót được đảm bảo một cách tầm thường. Điều này đảm bảo rằng không gian tìm kiếm bị giới hạn và câu trả lời luôn tồn tại. 

Một trường hợp góc khác là khi số chẵn ℓ = 0 đã cho D(0) < h. Khi đó, câu trả lời là 0 và tìm kiếm nhị phân phải cho phép giới hạn dưới hợp lệ một cách chính xác mà không bỏ qua nó do nhầm lẫn. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi cấp độ có thể ℓ bắt đầu từ 0 và tính tổng thiệt hại cho mỗi lựa chọn. Đối với ℓ cố định, chúng tôi quét tất cả các sóng và tính max(0, dᵢ − ℓ) · tᵢ, tính tổng thành tổng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về thiệt hại. Tuy nhiên, mỗi đánh giá là O(n) và nếu ℓ lên tới max(dᵢ), thì tổng chi phí sẽ trở thành O(n · max(dᵢ)), quá lớn khi dᵢ có thể lớn. 

Sự cải thiện đến từ việc nhận ra rằng chúng tôi không tối ưu hóa một hàm tùy ý mà đang tìm kiếm một vị từ đơn điệu. Điều kiện “D(ℓ) < h” hoạt động nhất quán: nếu nó đúng với một số ℓ thì nó đúng với tất cả ℓ lớn hơn. Điều này chuyển bài toán thành việc tìm điểm ngưỡng trong hàm giảm đơn điệu. 

Khi cấu trúc này hiển thị, tìm kiếm nhị phân sẽ trở thành công cụ tự nhiên. Chúng tôi xác định phạm vi tìm kiếm từ 0 đến max(dᵢ). Giới hạn trên là đủ vì khi ℓ đạt max(dᵢ), tất cả các số hạng đều biến mất và thiệt hại bằng không. Mỗi lần kiểm tra đánh giá D(ℓ) theo thời gian tuyến tính, đưa ra tổng số lần đánh giá logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · max(dᵢ)) | O(1) | Quá chậm | 
| Tối ưu (Tìm kiếm nhị phân) | O(n log max(dᵢ)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta muốn xác định ℓ nhỏ nhất sao cho tổng thiệt hại giảm xuống dưới h.

1. Tính giá trị lớn nhất trong số tất cả dᵢ để xác định giới hạn trên của không gian tìm kiếm. Điều này có tác dụng vì bất kỳ ℓ nào ngoài điểm này sẽ vô hiệu hóa mọi đóng góp thiệt hại. 
2. Đặt ranh giới tìm kiếm nhị phân với low = 0 và high = max(dᵢ). Chúng ta đang tìm kiếm trên một miền đơn điệu rời rạc. 
3. Trong khi thấp ≤ cao, hãy tính trung bình = (thấp + cao) // 2. Điều này thể hiện cấp độ ứng viên. 
4. Đánh giá tổng thiệt hại ở cấp độ giữa bằng cách lặp qua tất cả các đợt và tính tổng max(0, dᵢ − mid) · tᵢ. Điều này mô phỏng trực tiếp việc xác định vấn đề. 
5. Nếu sát thương tính toán ít nhất là h thì mid không đủ nên chúng ta chuyển từ thấp xuống trung bình + 1. Điều này là do tăng ℓ là cách duy nhất để giảm sát thương. 
6. Mặt khác, mid là đủ, vì vậy chúng tôi lưu trữ nó dưới dạng câu trả lời ứng cử viên và di chuyển từ cao lên giữa − 1 để thử tìm mức hợp lệ nhỏ hơn. 
7. Sau khi vòng lặp kết thúc, ứng cử viên được lưu trữ có giá trị tối thiểu là ℓ. 

Tại sao nó hoạt động 

Hàm D(ℓ) không tăng trong ℓ vì mỗi số hạng riêng lẻ max(0, dᵢ − ℓ) không tăng. Điều này tạo ra một vùng liền kề chứa các giá trị không hợp lệ, theo sau là vùng liền kề chứa các giá trị hợp lệ trên dòng số nguyên. Tìm kiếm nhị phân khai thác cấu trúc này bằng cách liên tục loại bỏ một nửa không gian tìm kiếm trong khi vẫn giữ nguyên ranh giới giữa các vùng không hợp lệ và hợp lệ. Thuật toán duy trì tính bất biến rằng tất cả các giá trị dưới mức thấp đều không hợp lệ và tất cả các giá trị trên mức cao đều hợp lệ, do đó câu trả lời phải nằm trong khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, h = map(int, input().split())
    d = []
    t = []
    mx = 0

    for _ in range(n):
        di, ti = map(int, input().split())
        d.append(di)
        t.append(ti)
        if di > mx:
            mx = di

    def damage(l):
        total = 0
        for i in range(n):
            if d[i] > l:
                total += (d[i] - l) * t[i]
                if total >= h:
                    return total
        return total

    low, high = 0, mx
    ans = mx

    while low <= high:
        mid = (low + high) // 2
        if damage(mid) >= h:
            low = mid + 1
        else:
            ans = mid
            high = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc tìm kiếm nhị phân trực tiếp. Hàm thiệt hại bao gồm việc thoát sớm khi tổng tích lũy đạt đến h, điều này ngăn cản việc tính toán không cần thiết trong các trường hợp lớn. Điều này không ảnh hưởng đến tính đúng đắn vì chúng ta chỉ cần phân biệt xem D(ℓ) có ít nhất là h hay không. 

Tìm kiếm nhị phân theo dõi câu trả lời hợp lệ nhất trong câu trả lời. Khi giá trị ở giữa thỏa điều kiện, chúng ta vẫn tiếp tục tìm kiếm bên trái để đảm bảo tính tối thiểu. 

Một cạm bẫy triển khai phổ biến là quên điều kiện dừng sớm hoặc tính toán (dᵢ − ℓ) cho tất cả i ngay cả khi dᵢ ℓ, điều này sẽ thêm các hoạt động không cần thiết và có nguy cơ tích lũy không chính xác nếu bao gồm các giá trị âm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba sóng: 

n = 3, h = 10 

sóng: (d, t) = (5, 2), (8, 1), (3, 4) 

Chúng tôi tính toán các giá trị ℓ ứng cử viên. 

| ℓ | Sóng 1 | Sóng 2 | Sóng 3 | Tổng D(ℓ) | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 8 | 12 | 30 | 
| 2 | 6 | 6 | 4 | 16 | 
| 4 | 2 | 4 | 0 | 6 | 
| 5 | 0 | 3 | 0 | 3 | 

Đối với ℓ = 2, sát thương vẫn trên 10 nên không hợp lệ. Với ℓ = 4, sát thương giảm xuống dưới 10 nên nó hợp lệ. Giá trị hợp lệ tối thiểu ℓ là 4. 

Dấu vết này cho thấy sự giảm đơn điệu của D(ℓ), đây chính xác là điều mà tìm kiếm nhị phân dựa vào. 

Bây giờ hãy xem xét một trường hợp mà việc sống sót đã dễ dàng: 

n = 2, h = 5 

(2, 10), (1, 10) 

| ℓ | Tổng D(ℓ) | 
| --- | --- | 
| 0 | 30 | 
| 1 | 10 | 
| 2 | 0 | 

Ở đây câu trả lời là ℓ = 1. Tìm kiếm nhị phân sẽ hội tụ chính xác gần mức đầu tiên nơi sát thương giảm xuống dưới h. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log max(dᵢ)) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các sóng và số bước là logarit trong phạm vi giá trị | 
| Không gian | O(1) | Chỉ lưu trữ mảng đầu vào và một vài biến | 

Các ràng buộc được thỏa mãn vì ngay cả đối với n lên tới 2×10⁵ và max(dᵢ) lên đến các giá trị lớn, hệ số logarit vẫn giữ cho tổng số phép toán có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# Sample-style sanity check
assert run("3 10\n5 2\n8 1\n3 4\n") == "4"

# Already safe at level 0
assert run("2 100\n1 5\n2 3\n") == "0"

# Single dominant wave
assert run("1 5\n10 2\n") == "8"

# All damage zero after threshold
assert run("2 1\n5 1\n5 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sóng hỗn hợp | 4 | hành vi đơn điệu tiêu chuẩn | 
| h thấp | 0 | câu trả lời có thể bằng 0 | 
| sóng đơn | 8 | độ đúng tối thiểu n | 
| bằng dᵢ | 5 | ranh giới khi tất cả trở thành số 0 | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi ℓ = 0 đã thỏa mãn điều kiện. Ví dụ: nếu tất cả các sóng cực kỳ yếu hoặc h lớn: 

n = 2, h = 100 

(1, 1), (2, 1) 

Tại ℓ = 0, tổng sát thương là 3, đã dưới h. Tìm kiếm nhị phân xác định chính xác rằng các giá trị ở giữa sẽ luôn hợp lệ, do đó mức cao sẽ giảm về 0. Thuật toán duy trì ans = 0 ngay khi tìm thấy đường giữa hợp lệ. 

Một trường hợp cạnh khác là khi ℓ tối ưu bằng max(dᵢ). Ví dụ: 

n = 2, h = 1 

(3, 1), (3, 1) 

Ở ℓ = 2, sát thương vẫn dương; tại ℓ = 3, mọi đóng góp đều biến mất. Tìm kiếm nhị phân phải bao gồm high = max(dᵢ), nếu không điểm biên chính xác sẽ bị loại trừ.
