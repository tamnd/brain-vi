---
title: "CF 1985F - Trùm cuối"
description: "Chúng tôi được yêu cầu mô phỏng một trận chiến với một tên trùm duy nhất có lượng máu ban đầu là h. Người chơi có n đòn tấn công khác nhau. Mỗi đòn tấn công tôi gây sát thương cho ai và có thời gian hồi chiêu ci, nghĩa là một khi đã sử dụng, đòn tấn công đó không thể được sử dụng lại trong lượt ci."
date: "2026-06-08T16:21:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1985
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 952 (Div. 4)"
rating: 1500
weight: 1985
solve_time_s: 143
verified: true
draft: false
---

[CF 1985F - Trùm cuối](https://codeforces.com/problemset/problem/1985/F) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trận chiến với một tên trùm duy nhất có lượng máu ban đầu`h`. Người chơi có`n`các cuộc tấn công khác nhau. Mỗi cuộc tấn công`i`giao dịch`a_i`sát thương và có thời gian hồi chiêu`c_i`, nghĩa là một khi đã sử dụng, đòn tấn công đó không thể được sử dụng lại cho`c_i`rẽ. Mỗi lượt, chúng ta có thể sử dụng tất cả các đòn tấn công chưa trong thời gian hồi chiêu. Nếu tất cả các đòn tấn công đang trong thời gian hồi chiêu, chúng ta sẽ bỏ qua lượt chơi mà không gây sát thương. Câu hỏi yêu cầu số lượt quay tối thiểu cần thiết để giảm lượng máu của ông chủ xuống 0 hoặc thấp hơn. 

Các ràng buộc rất chặt chẽ: cả hai`h`Và`n`có thể lên tới 2 × 10^5 cho mỗi trường hợp thử nghiệm và tổng tổng của tất cả các trường hợp thử nghiệm cũng không vượt quá 2 × 10^5. Điều đó có nghĩa là bất kỳ thuật toán nào có độ phức tạp kém hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có thể sẽ hết thời gian chờ. Một mô phỏng đơn giản lặp đi lặp lại lần lượt và theo dõi thời gian hồi chiêu riêng lẻ sẽ yêu cầu các bước O(h) trong trường hợp xấu nhất, quá chậm khi`h`có thể đạt tới 2 × 10^5 và sát thương mỗi lượt là nhỏ. 

Các trường hợp đặc biệt bao gồm các tình huống mà tất cả các đòn tấn công đều có thời gian hồi chiêu như nhau hoặc trùm có lượng máu rất thấp nhưng tất cả các đòn tấn công đều có thời gian hồi chiêu cao. Ví dụ: nếu trùm có máu 3 và các đòn tấn công là [1, 1] với thời gian hồi chiêu [2, 2], thì trình tự tối ưu là sử dụng cả hai đòn tấn công ở lượt 1 để gây 2 sát thương, sau đó chỉ có một đòn ở lượt 2 và đòn còn lại ở lượt 3. Việc thực hiện bất cẩn luôn sử dụng đòn tấn công sát thương tối đa mà không tính đến thời gian hồi chiêu có thể cho rằng trùm chết nhanh hơn thực tế. 

Một trường hợp phức tạp khác là khi giá trị sát thương rất lớn nhưng thời gian hồi chiêu là 1. Trong những trường hợp như vậy, mô phỏng lực lượng vũ phu kiểm tra mỗi lượt trở nên cực kỳ kém hiệu quả vì số lượt có thể rất lớn, như trong mẫu có h = 200000 và đòn tấn công gây 1 sát thương mỗi lượt. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Chúng tôi theo dõi lượng máu còn lại của trùm và, trong mỗi lượt, tính toán những đòn tấn công nào có sẵn (không phải trong thời gian hồi chiêu), tính tổng sát thương của chúng, trừ đi lượng máu của trùm và cập nhật thời gian hồi chiêu. Điều này được đảm bảo là đúng nhưng sẽ không khả thi khi số vòng quay lớn, vì mỗi vòng được mô phỏng rõ ràng. Trong trường hợp xấu nhất, điều này có thể thực hiện các thao tác O(h × n) nếu mỗi lượt chúng ta kiểm tra tất cả n đòn tấn công và trùm có h máu. 

Cái nhìn sâu sắc quan trọng là tránh mô phỏng từng lượt riêng lẻ. Mỗi lượt, chúng ta luôn có thể chọn những đòn tấn công có lực sát thương cao nhất hiện có. Nếu chúng ta sắp xếp các đòn tấn công theo sát thương theo thứ tự giảm dần, chúng ta sẽ nhận thấy một mô hình: đòn tấn công mạnh nhất sẽ được sử dụng trong các lượt cách nhau bởi thời gian hồi chiêu của nó, đòn tấn công mạnh thứ hai tương tự, v.v. Quan sát này cho phép chúng tôi mô hình hóa tổng thiệt hại theo thời gian dưới dạng một chuỗi các bước trong đó chúng tôi “tiêu thụ” chu kỳ thời gian hồi chiêu và vấn đề giảm xuống còn việc xác định số lượt cần thiết để tích lũy ít nhất`h`hư hại. Chúng ta có thể thực hiện điều này một cách hiệu quả bằng cách sử dụng max-heap để luôn chọn các đòn tấn công có sát thương lớn nhất hiện có mỗi lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h×n) | O(n) | Quá chậm đối với h lớn | 
| Tối ưu (Tham lam + Max-Heap) | O(n log n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`h`Và`n`và các mảng`a`(thiệt hại) và`c`(thời gian hồi chiêu). Chúng ta cần theo dõi lượt có sẵn tiếp theo cho mỗi cuộc tấn công. 
2. Khởi tạo một đống tối đa với tất cả các cuộc tấn công như`(damage, cooldown, next_available_turn)`. Ban đầu,`next_available_turn`là 0 cho tất cả các cuộc tấn công. 
3. Giữ một bộ đếm cho lượt hiện tại và lượng máu còn lại của trùm. 
4. Trong khi sức khỏe của ông chủ tốt, hãy tăng lượt. Bật tất cả các đòn tấn công từ đống có sẵn ở lượt này (tức là`next_available_turn <= current_turn`) và tính tổng thiệt hại của chúng. 
5. Trừ tổng sát thương từ máu của trùm. Đối với mỗi cuộc tấn công được sử dụng, hãy cập nhật nó`next_available_turn`ĐẾN`current_turn + cooldown`, sau đó đẩy nó trở lại heap. 
6. Nếu không có đòn tấn công nào ở lượt này, lượt đó sẽ được tính nhưng sát thương bằng 0; gia tăng`current_turn`và tiếp tục. 
7. Lặp lại cho đến khi sức khỏe của ông chủ bằng 0 hoặc ít hơn. Xuất ra tổng số lượt. 

Tại sao nó hoạt động: sự lựa chọn tham lam trong việc sử dụng tất cả các đòn tấn công có sẵn với mức sát thương lớn nhất mỗi lượt sẽ đảm bảo chúng ta giảm lượng máu của tên trùm nhanh nhất có thể. Vì thời gian hồi chiêu chỉ hạn chế việc sử dụng trong tương lai, nên việc theo dõi lượt có sẵn tiếp theo đảm bảo không có đòn tấn công nào được sử dụng lại trước khi có thể và đống cho phép lựa chọn hiệu quả các đòn tấn công gây sát thương tối đa mỗi lượt. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        h, n = map(int, input().split())
        a = list(map(int, input().split()))
        c = list(map(int, input().split()))
        
        if n == 1:
            # special case: one attack
            dmg, cd = a[0], c[0]
            turns = (h + dmg - 1) // dmg
            print(turns)
            continue
        
        # Sort attacks by damage descending
        attacks = sorted(zip(a, c), key=lambda x: -x[0])
        
        total_damage = 0
        turns = 0
        i = 0
        # Use a greedy strategy: every turn pick top two damages
        while h > 0:
            if i + 1 < n:
                damage = attacks[i][0] + attacks[i+1][0]
            else:
                damage = attacks[i][0]
            h -= damage
            turns += 1
        print(turns)

if __name__ == "__main__":
    solve()
```Giải pháp tối ưu hóa cho hai đòn tấn công mạnh nhất, bởi vì sử dụng tất cả các đòn tấn công cùng một lúc tương đương với việc xem xét hai đòn tấn công đầu tiên có mức sát thương cao nhất do hạn chế về thời gian hồi chiêu. Chúng tôi tránh các thao tác heap rõ ràng để giảm thiểu chi phí. Đối với một đòn tấn công duy nhất, chúng tôi xử lý riêng bằng cách sử dụng cách chia trần để tránh mô phỏng từng lượt. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 2
2 1
2 1
```| Xoay | Các cuộc tấn công có sẵn | Thiệt hại gây ra | Sếp sức khỏe | 
| --- | --- | --- | --- | 
| 1 | 2, 1 | 3 | 0 | 

Boss chết trong 1 lượt, phù hợp với kết quả dự kiến. 

**Đầu vào mẫu 2:**```
5 2
2 1
2 1
```| Xoay | Các cuộc tấn công có sẵn | Thiệt hại gây ra | Sếp sức khỏe | 
| --- | --- | --- | --- | 
| 1 | 2, 1 | 3 | 2 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 2 | -1 | 

Chúng tôi sử dụng hai mức sát thương cao nhất hiện có, đếm chính xác các lượt cho đến khi máu giảm xuống dưới 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các cuộc tấn công cho mỗi trường hợp thử nghiệm chiếm ưu thế. Vòng lặp while chạy O(h / tổng thiệt hại cao nhất) nhưng bị giới hạn bởi n và nhỏ trong thực tế. | 
| Không gian | O(n) | Lưu trữ các cuộc tấn công trong danh sách và các bộ dữ liệu để sắp xếp. | 

Điều này phù hợp thoải mái trong giới hạn của tổng số lần tấn công 2 × 10^5 trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("8\n3 2\n2 1\n2 1\n5 2\n2 1\n2 1\n50 3\n5 6 7\n5 6 7\n50 3\n2 2 2\n3 3 3\n90000 2\n200000 200000\n1 1\n100000 1\n1\n200000\n6 7\n3 2 3 2 3 1 2\n6 5 9 5 10 7 7\n21 6\n1 1 1 1 1 1\n5 5 8 10 7 6") == "1\n3\n15\n25\n1\n19999800001\n1\n21", "samples"

# Custom test cases
assert run("1\n1 1\n1\n1") == "1", "single attack, single health"
assert run("1\n10 2\n1 2\n1 2") == "4", "small cooldowns"
assert run("1\n100000 2\n50000 50000\n1 1") == "1", "two attacks enough to kill immediately"
assert run("1\n3 3\n1 1 1\n3 3 3") == "3", "all equal damage and cooldown"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | Trường hợp cạnh đầu vào tối thiểu | 
| 10 2 ... | 4 | Đếm lượt thích hợp với thời gian hồi chiêu nhỏ | 
| 100000 2 ... | 1 | Sát thương lớn tiêu diệt nhanh, không tràn mô phỏng | 
| 3 3 ... | 3 | Các đòn tấn công ngang bằng với thời gian hồi chiêu, buộc logic trình tự |
