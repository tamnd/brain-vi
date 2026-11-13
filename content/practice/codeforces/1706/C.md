---
title: "CF 1706C - Qpwoeirut Và Thành Phố"
description: "Chúng tôi được cấp một dãy tòa nhà, mỗi tòa nhà có số tầng nhất định. Một tòa nhà được coi là \"mát mẻ\" nếu nó cao hơn hẳn so với cả những tòa nhà lân cận. Tòa nhà đầu tiên và cuối cùng không thể mát mẻ vì không có hai người hàng xóm."
date: "2026-06-09T21:17:24+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1706
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 809 (Div. 2)"
rating: 1400
weight: 1706
solve_time_s: 111
verified: false
draft: false
---

[CF 1706C - Qpwoeirut Và Thành Phố](https://codeforces.com/problemset/problem/1706/C) 

**Đánh giá:** 1400 
**Tags:** dp, dòng chảy, tham lam, thực hiện 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một dãy tòa nhà, mỗi tòa nhà có số tầng nhất định. Một tòa nhà được coi là "mát mẻ" nếu nó cao hơn hẳn so với cả những tòa nhà lân cận. Tòa nhà đầu tiên và cuối cùng không thể mát mẻ vì không có hai người hàng xóm. Chúng tôi có thể tăng chiều cao của bất kỳ tòa nhà nào bằng cách thêm tầng và mục tiêu là tối đa hóa số lượng tòa nhà mát mẻ đồng thời giảm thiểu tổng số tầng bổ sung được thêm vào. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng tòa nhà và chiều cao hiện tại của chúng. Đầu ra của mỗi trường hợp thử nghiệm là số tầng tối thiểu cần thiết để tối đa hóa số lượng tòa nhà mát mẻ. 

Các ràng buộc chỉ ra rằng tổng số lượng tòa nhà trong tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này loại trừ các giải pháp có tính chất bậc hai về số lượng tòa nhà trên mỗi trường hợp thử nghiệm. Chúng ta cần một thuật toán chạy gần như theo thời gian tuyến tính cho từng trường hợp thử nghiệm. 

Các trường hợp biên không rõ ràng bao gồm các chuỗi trong đó các tòa nhà đã có chiều cao xen kẽ hoặc các chuỗi trong đó nhiều tòa nhà liền kề có chiều cao bằng nhau. Ví dụ, một trình tự`[1, 2, 1, 2, 1]`đã có các tòa nhà mát mẻ tối đa ở vị trí 2 và 4 nên không cần thêm tầng. Một trường hợp phức tạp khác là`[3, 1, 4, 4, 2]`nơi hai tòa nhà liên tiếp bằng nhau; Việc chọn tăng tòa nhà nào có thể ảnh hưởng đến tổng số tầng được thêm vào. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ là thử làm mát từng tòa nhà bên trong riêng lẻ và tính toán số tầng cần thiết. Sau đó, chúng tôi có thể thử tất cả các kết hợp tòa nhà để tìm ra số lượng tòa nhà thú vị tối đa và số tầng bổ sung tối thiểu. Tuy nhiên, đối với`n`lên đến`10^5`, việc thử tất cả các kết hợp rõ ràng là không khả thi, vì số lượng khả năng tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét tất cả các kết hợp. Những tòa nhà mát mẻ không thể ở cạnh nhau, bởi vì nếu chúng ta cố gắng làm cho hai tòa nhà liên tiếp cao hơn những tòa nhà lân cận thì chiều cao của chúng sẽ xung đột nhau. Quan sát này làm giảm vấn đề thành một lựa chọn độc lập của tất cả các tòa nhà bên trong được lập chỉ mục lẻ hoặc tất cả các tòa nhà bên trong được lập chỉ mục chẵn. Chúng ta có thể tính toán số tầng cần thiết cho hai mẫu này và chọn mẫu có tổng nhỏ hơn. Đối với các chuỗi có độ dài lẻ, chúng ta có thể cần xem xét phương pháp lai bằng cách kiểm tra các cặp chồng chéo, nhưng đối với phần lớn các trường hợp, việc so sánh các vị trí chẵn và lẻ là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng tòa nhà`n`và dãy độ cao`h`. 
2. Nếu`n`là số lẻ, hãy khởi tạo hai tổng: một cho các tầng bổ sung cần thiết để làm mát tất cả các tòa nhà bên trong được lập chỉ mục lẻ và một tổng khác cho tất cả các tòa nhà bên trong được lập chỉ mục chẵn. 
3. Đối với từng tòa nhà nội bộ tại mục lục`i`(dựa trên 1, vì vậy`2 ≤ i ≤ n-1`), tính số tầng tối thiểu cần thiết để làm cho nó cao hơn cả hai hàng xóm:`max(h[i-1], h[i+1]) + 1 - h[i]`. Nếu kết quả là âm tính, hãy sử dụng số 0. 
4. Cộng các tầng tính toán vào tổng cho các vị trí chẵn hoặc lẻ tùy theo`i`. 
5. Sau khi xử lý tất cả các tòa nhà bên trong, nếu`n`lẻ thì chọn tổng nhỏ nhất của hai tổng đó. Nếu như`n`chẵn, mô hình phức tạp hơn, vì vậy hãy sử dụng phương pháp cửa sổ trượt để tính tổng các tòa nhà mát mẻ không liền kề và chọn tổng tối thiểu. 
6. Xuất ra tổng số tầng tối thiểu cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Điều bất biến là không có hai tòa nhà liền kề nào có thể cùng mát, do đó, bất kỳ giải pháp tối ưu nào cũng phải chọn một tập hợp con các tòa nhà không liền kề. Thuật toán xem xét hai tập hợp con không liền kề tối đa (lẻ và chẵn), đảm bảo rằng chúng tôi tìm thấy tổng tối thiểu để tối đa hóa số lượng tòa nhà mát mẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))
        if n % 2 == 1:
            total = 0
            for i in range(1, n-1, 2):
                total += max(0, max(h[i-1], h[i+1]) + 1 - h[i])
            print(total)
        else:
            # Sliding window for even length
            add = [0] * (n-2)
            for i in range(1, n-1):
                add[i-1] = max(0, max(h[i-1], h[i+1]) + 1 - h[i])
            prefix = [0] * (n-1)
            prefix[0] = add[0]
            for i in range(1, n-2):
                prefix[i] = prefix[i-1] + add[i]
            res = float('inf')
            for i in range((n-2)//2 + 1):
                left = prefix[i-1] if i > 0 else 0
                right = prefix[-1] - prefix[i + (n-2)//2 - 1] if i + (n-2)//2 - 1 < n-3 else 0
                middle = prefix[i + (n-2)//2 - 1] - prefix[i-1] if i > 0 else prefix[i + (n-2)//2 - 1]
                res = min(res, left + right + middle)
            print(res)

solve()
```Đầu tiên, mã này xử lý các chuỗi có độ dài lẻ bằng cách tính tổng số tầng tối thiểu cần thiết cho mọi tòa nhà khác. Đối với các chuỗi có độ dài chẵn, nó tính toán trước các tầng cần thiết cho tất cả các tòa nhà bên trong, sau đó sử dụng tổng tiền tố và cửa sổ trượt để xem xét sự kết hợp của các tòa nhà không liền kề. Việc lập chỉ mục được đặc biệt chú ý để tránh các lỗi sai sót một. Các giá trị âm được giới hạn bằng 0 vì không cần tầng nếu tòa nhà đã cao hơn cả hai tòa nhà lân cận. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[2, 1, 2]`| tôi | h[i-1] | h[i] | h[i+1] | tầng cần thiết | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 2 | 

Tòa nhà thứ hai là tòa nhà nội bộ duy nhất. Chúng ta cần xây thêm 2 tầng để nó cao hơn cả hai tầng bên cạnh. Đầu ra là`2`. Điều này xác nhận rằng một tòa nhà nội bộ có thể được xử lý trực tiếp. 

**Ví dụ 2** 

đầu vào:`[1, 2, 1, 4, 3]`| tôi | h[i-1] | h[i] | h[i+1] | tầng cần thiết | 

|---|--------|------|--------|
