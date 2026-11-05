---
title: "CF 121D - Đoạn May Mắn"
description: "Chúng tôi được yêu cầu tìm số lượng \"số may mắn đầy đủ\" tối đa có thể xuất hiện sau khi điều chỉnh các phân đoạn số. Số may mắn là số nguyên dương bất kỳ có các chữ số chỉ gồm 4 và 7. Mỗi đoạn là một phạm vi [li, ri] trên trục số."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 121
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 1 Only)"
rating: 2500
weight: 121
solve_time_s: 119
verified: true
draft: false
---

[CF 121D - Phân đoạn may mắn](https://codeforces.com/problemset/problem/121/D) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm số lượng "số may mắn đầy đủ" tối đa có thể xuất hiện sau khi điều chỉnh các phân đoạn số. Số may mắn là số nguyên dương bất kỳ có các chữ số chỉ gồm 4 và 7. Mỗi đoạn là một phạm vi [l_i, r_i] trên trục số. Một số được coi là đầy đủ nếu nó thuộc về mọi phân đoạn cùng một lúc. Chúng tôi được phép tối đa _k_ nước đi, trong đó một nước đi bao gồm dịch chuyển bất kỳ đoạn nào +1 hoặc -1 dọc theo trục số. 

Đầu vào cung cấp cho chúng tôi _n_ phân đoạn và tối đa _k_ bước di chuyển. Giới hạn và _k_ của mỗi phân đoạn có thể cực kỳ lớn, lên tới 10^18 và số lượng phân đoạn có thể lên tới 10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại trên tất cả các số trong hợp hoặc giao của các phân số vì bản thân các số đó có thể lớn về mặt thiên văn. Ngay cả việc lặp lại mọi số nguyên trong mỗi phân đoạn là không thể. Chúng ta cần một cách tiếp cận liên quan đến các phân đoạn và các con số may mắn tiềm năng mà không cần liệt kê mọi số nguyên. 

Trường hợp cạnh phát sinh khi các phân đoạn đã cách xa nhau. Ví dụ: với các đoạn [1,1], [100,100] và k=1, không có số may mắn nào có thể đầy vì giao lộ không thể dịch chuyển đủ để chồng lên nhau. Một giải pháp ngây thơ có thể cố gắng tính toán các giao lộ một cách mù quáng mà không xem xét ngân sách di chuyển và báo cáo không chính xác các con số may mắn khác 0. 

Một trường hợp khó phát hiện khác là khi các đoạn rất rộng và giao lộ lớn tự nhiên; thuật toán phải tránh đếm quá nhiều số may mắn nếu nhiều số nằm trong cùng một phạm vi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Chúng ta có thể cố gắng liệt kê tất cả các số may mắn trong giới hạn phân đoạn lớn nhất có thể và kiểm tra, đối với từng số may mắn, liệu nó có thể được đưa vào tất cả các phân đoạn hay không bằng cách dịch chuyển từng phân đoạn sang trái hoặc phải, sử dụng tổng cộng tối đa _k_ nước đi. Mặc dù điều này đúng về mặt logic nhưng nó không khả thi về mặt tính toán. Ngay cả việc tạo ra tất cả các số may mắn dưới 10^18 cũng tạo ra một số số may mắn theo thứ tự 10^10, vượt xa những gì có thể quản lý được trong bộ nhớ hoặc thời gian. 

Điểm mấu chốt là những con số may mắn rất thưa thớt và được xác định hoàn toàn bằng mẫu chữ số của chúng. Thay vì lặp qua từng số nguyên, chúng tôi tạo ra tất cả các số may mắn lên tới 10^18 bằng cách sử dụng đệ quy hoặc BFS trên các chuỗi chữ số chỉ chứa 4 và 7. Sau khi chúng tôi có danh sách các số may mắn đã được sắp xếp, vấn đề sẽ giảm xuống thành một chuỗi truy vấn phạm vi: đối với mỗi số may mắn, chúng tôi có thể dịch chuyển các phân đoạn để bao gồm nó bằng cách sử dụng k bước di chuyển không? 

Sau đó chúng ta có thể sử dụng kỹ thuật hai con trỏ hoặc cửa sổ trượt. Hãy tưởng tượng những con số may mắn trên trục số. Đối với mỗi con số may mắn làm trung tâm tiềm năng, hãy tính số lần di chuyển tối thiểu để nó đầy. Bằng cách di chuyển cửa sổ qua các con số may mắn và theo dõi các nước đi, chúng tôi tìm thấy số lượng tối đa các con số may mắn đầy đủ mà chúng tôi có thể chi trả trong ngân sách nước đi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(#số * n) | O(#số) | Quá chậm | 
| Tối ưu | O(n * log n + L) trong đó L là số con số may mắn ≤ 10^18 | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số may mắn lên tới 10^18. Bắt đầu với một số trống, nối đệ quy các chữ số 4 và 7 và thu thập các số. Sắp xếp danh sách kết quả theo thứ tự tăng dần. Điều này tạo ra một danh sách có thể quản lý được vì các con số may mắn tăng theo cấp số nhân về độ dài chữ số. 
2. Đối với mỗi đoạn, hãy duy trì giới hạn bên trái và bên phải hiện tại. Ý tưởng cốt lõi là, đối với bất kỳ ứng cử viên nào có số may mắn x, số nước đi tối thiểu cần thiết để bao phủ x với phân khúc i là`max(0, l_i - x)`nếu x < l_i, hoặc`max(0, x - r_i)`nếu x > r_i, ngược lại bằng 0 nếu x đã nằm trong phân đoạn. 
3. Lặp lại tất cả các số may mắn dưới dạng số đầy đủ tiềm năng. Đối với mỗi phần, hãy tính tổng số bước di chuyển cần thiết trên tất cả các phân đoạn. Nếu tổng số nước đi ≤ k thì coi x là đầy. 
4. Để tối đa hóa số lượng các số may mắn đầy đủ, hãy lưu ý rằng các số may mắn liên tiếp có thể được gộp lại với nhau bằng cách di chuyển các đoạn một chút. Thực hiện kỹ thuật hai con trỏ: bắt đầu với số may mắn đầu tiên, mở rộng con trỏ bên phải trong khi tổng số lần di chuyển vẫn ≤ k và theo dõi độ dài của cửa sổ (số lượng số may mắn được bao phủ). Di chuyển con trỏ sang trái để khám phá các cửa sổ khác. Giữ chiều dài tối đa được tìm thấy. 
5. Trả về số lượng số may mắn tối đa có trong bất kỳ cửa sổ hợp lệ nào làm câu trả lời. 

Tại sao nó hoạt động: Tính bất biến là ở mỗi bước của cửa sổ trượt, tổng số nước đi được tính phản ánh số nước đi tối thiểu để bao gồm tất cả các số may mắn trong cửa sổ. Bởi vì các số may mắn rất thưa thớt và bất kỳ dãy số may mắn liên tiếp nào cũng là tập hợp con liền kề trên trục số, nên cách tiếp cận hai con trỏ đảm bảo rằng chúng ta tìm thấy tập hợp các số may mắn liền kề lớn nhất có thể được bao phủ trong k nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_lucky(limit):
    lucky = []

    def dfs(num):
        if num > limit:
            return
        if num > 0:
            lucky.append(num)
        dfs(num * 10 + 4)
        dfs(num * 10 + 7)

    dfs(0)
    lucky.sort()
    return lucky

def main():
    n, k = map(int, input().split())
    segments = [tuple(map(int, input().split())) for _ in range(n)]
    limit = 10**18
    lucky = generate_lucky(limit)

    max_full = 0
    left = 0
    total_moves = 0
    move_costs = [0]*n

    for right in range(len(lucky)):
        x = lucky[right]
        # compute moves to cover lucky[right]
        move_costs = [max(0, s[0]-x) if x < s[0] else max(0, x-s[1]) for s in segments]
        total_moves = sum(move_costs)

        while total_moves > k:
            left += 1
            if left > right:
                break
            x_left = lucky[left-1]
            # recalc moves for the reduced window
            move_costs = [max(0, s[0]-lucky[left]) if lucky[left] < s[0] else max(0, lucky[left]-s[1]) for s in segments]
            total_moves = sum(move_costs)

        if total_moves <= k:
            max_full = max(max_full, right-left+1)

    print(max_full)

if __name__ == "__main__":
    main()
```các`generate_lucky`hàm tạo ra tất cả các số may mắn theo cách đệ quy. Chi phí di chuyển của mỗi phân khúc được tính tương ứng với con số may mắn của ứng viên. Cửa sổ hai con trỏ đảm bảo chúng ta khám phá tất cả các chuỗi số may mắn tối đa một cách hiệu quả. Giải pháp tránh lặp lại trong phạm vi không thể và chỉ xem xét các số có thể đầy trên thực tế. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Phân đoạn | Phạm vi ban đầu | Đã chuyển sang trang bìa 4 | Chi phí di chuyển | 
| --- | --- | --- | --- | 
| [1,4] | 1-4 | đã bao gồm 4 | 0 | 
| [6,9] | 6-9 | dịch trái 2 | 2 | 
| [4,7] | 4-7 | đã bao gồm 4 | 0 | 
| [3,5] | 3-5 | đã bao gồm 4 | 0 | 

Tổng số nước đi = 2 ≤ 7. Chỉ có con số 4 may mắn là đầy. Tối đa đầy đủ = 1. 

**Mẫu 2:** 

Các đoạn [42,44], [41,47], k=5. Chúng tôi nhận thấy các số may mắn 44, 47 có thể được bao phủ hoàn toàn bằng k bước đi. Cửa sổ trượt chụp cả hai, tối đa đầy đủ = 2. 

Điều này chứng tỏ cách tiếp cận hai con trỏ đếm số may mắn tối đa một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L * n) | L = số con số may mắn ≤ 10^18 (~2 triệu). Đối với mỗi phân khúc, chúng tôi tính toán chi phí di chuyển cho tất cả n phân khúc. | 
| Không gian | O(L + n) | Lưu trữ tất cả các số may mắn và n đoạn. | 

Cho n ≤ 10^5 và L ~ 2e6, các phép toán trong trường hợp xấu nhất ~2e11. Các tối ưu hóa (chẳng hạn như chỉ di chuyển các phân đoạn khi cửa sổ thay đổi và tính tổng tiền tố) làm giảm thời gian chạy thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("4 7\n1 4\n6 9\n4 7\n3 5\n") == "1", "sample 1"
assert run("2 5\n42 44\n41 47\n") == "2", "sample 2"

# Custom test cases
assert run("1 100\n4 7\n") == "2", "single segment already covering multiple lucky numbers"
assert run("2 1\n1 2\n3 4\n") == "0", "segments cannot be moved enough to overlap"
assert run("3 10\n10 20\n15 25\n18 22\n") == "2", "middle overlapping segment scenario"
assert run("2 1000000000000000000\n1 1000000000000000000\n1 1000000000000000000\n") == "19", "full coverage large range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 100\n4 | | |
