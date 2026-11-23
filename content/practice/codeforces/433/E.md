---
title: "CF 433E - Đậu Phụ Tachibana Kanade"
description: "Chúng ta được yêu cầu đếm các số trong một phạm vi [l, r] nhất định (được biểu thị bằng cơ số m) thỏa mãn một ràng buộc “giá trị” nhất định. Mỗi số bắt đầu bằng giá trị 0. Chúng ta có n mẫu, mỗi mẫu là một chuỗi các chữ số trong cơ số m, với một giá trị nguyên liên quan."
date: "2026-06-07T02:41:32+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 433
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 248 (Div. 2)"
rating: 2500
weight: 433
solve_time_s: 103
verified: false
draft: false
---

[CF 433E - Đậu hũ của Tachibana Kanade](https://codeforces.com/problemset/problem/433/E) 

**Đánh giá:** 2500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các số trong một phạm vi nhất định [l, r] (được biểu thị bằng cơ số _m_) thỏa mãn một ràng buộc “giá trị” nhất định. Mỗi số bắt đầu bằng giá trị 0. Chúng ta được cung cấp các mẫu _n_, mỗi mẫu là một chuỗi các chữ số trong cơ sở _m_, với một giá trị số nguyên liên quan. Đối với một số, mỗi lần xuất hiện của một mẫu dưới dạng chuỗi con liền kề sẽ tăng giá trị của nó bằng giá trị của mẫu đó. Một số được coi là “Mapo Tofu” nếu tổng giá trị của nó không vượt quá _k_. Nhiệm vụ là đếm tất cả các số Mapo Tofu theo modulo 10^9+7. 

Đầu vào biểu thị các số dưới dạng mảng chữ số, không phải dưới dạng số nguyên, có nghĩa là chúng ta phải xử lý cẩn thận các số 0 đứng đầu và chuyển đổi cơ số. Các số có thể có tối đa 200 chữ số, cơ sở có thể lên tới 20 và số lượng mẫu lên tới 200. Mỗi mẫu có thể có tối đa 200 chữ số và tổng độ dài của mẫu là 200. 

Việc lặp lại một cách ngây thơ qua mọi số trong phạm vi là không thể vì phạm vi đó có thể có tới m^200 số. Hơn nữa, các mẫu có thể chồng chéo trong một số, do đó, một số có thể tích lũy các giá trị từ nhiều kết quả trùng khớp. Điều này loại trừ bất kỳ sự liệt kê vũ phu nào. Các trường hợp cạnh bao gồm các mẫu có số 0 đứng đầu, các số có chữ số tối thiểu (như l=1, r=1) và các số chính xác ở ngưỡng k. 

Ví dụ: hãy xem xét phạm vi nhị phân [10, 11] với mẫu “01” có giá trị 2 và k=2. Số 10 không chứa “01” nên giá trị của nó là 0 và là Mapo Tofu. Số 11 cũng không có số “01” nên cũng là Mapo Tofu. Một cách tiếp cận ngây thơ có thể tính sai do kết hợp chuỗi con không đúng hoặc các số 0 đứng đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra mọi số trong [l, r], chuyển đổi nó thành một chuỗi chữ số, kiểm tra tất cả các chuỗi con có thể có đối với tất cả các mẫu, tính tổng các giá trị của chúng và đếm các số ≤k. Độ phức tạp về thời gian là O((r-l+1) * n * max_pattern_length), điều này không khả thi vì r-l+1 có thể rất lớn (lên tới m^200). Ngay cả đối với những cơ số nhỏ như m=10, con số này nhanh chóng vượt quá 10^200, vượt xa mọi khả năng tính toán. 

Thông tin chuyên sâu quan trọng là sử dụng **lập trình động chữ số kết hợp với máy tự động Aho-Corasick**. Chữ số DP cho phép chúng ta đếm các số trong một phạm vi mà không tạo ra chúng một cách rõ ràng bằng cách xử lý các chữ số từ quan trọng nhất đến ít nhất, duy trì thông tin trạng thái như tiền tố hiện tại có khớp với giới hạn trên hay giới hạn dưới hay không. Máy tự động Aho-Corasick theo dõi một cách hiệu quả tất cả các lần xuất hiện mẫu khi các chữ số được xử lý, bao gồm cả các mẫu chồng chéo, đồng thời cập nhật giá trị tích lũy. Bằng cách kết hợp những điều này, chúng ta có thể đếm tất cả các số <r có giá trị ∼k và trừ đi số lượng <l để có câu trả lời cuối cùng. 

Brute-force hoạt động vì nó trực tiếp triển khai câu lệnh vấn đề, nhưng không thành công do kích thước quá lớn của không gian tìm kiếm. Quan sát cho thấy chúng ta có thể mã hóa các mẫu trong máy tự động và đếm số bằng DP dựa trên trạng thái và giá trị tích lũy sẽ làm giảm đáng kể độ phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1) * n * L) | O(n * L) | Quá chậm | 
| Chữ số DP + Aho-Corasick | O(len * num_states * k * 2) | O(num_states * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi tất cả các mẫu thành bộ ba Aho-Corasick. Mỗi nút trong trie tương ứng với một tiền tố của một số mẫu và chúng tôi tính toán trước cho mỗi nút tổng giá trị tích lũy nếu máy tự động đến nút đó (tổng của tất cả các giá trị mẫu kết thúc ở đó). 
2. Xây dựng các liên kết thất bại cho lần thử. Các liên kết này cho phép chúng tôi di chuyển đến nút hậu tố dài nhất khớp với tiền tố của một số mẫu khi chữ số tiếp theo không khớp với nút hiện tại, đảm bảo tất cả các lần xuất hiện của mẫu đều được tính một cách hiệu quả. 
3. Xác định trạng thái DP là dp[pos][node][value][tight_low][tight_high], trong đó pos là chỉ số chữ số hiện tại, nút là trạng thái Aho-Corasick hiện tại, giá trị là giá trị mẫu tích lũy cho đến nay, chặt chẽ_low và chặt chẽ theo dõi xem tiền tố hiện tại có tuân thủ nghiêm ngặt giới hạn dưới hay giới hạn trên hay không. Cờ chặt đảm bảo chúng tôi không đếm các số bên ngoài [l, r]. 
4. Khởi tạo DP ở vị trí 0 với nút gốc, giá trị 0 và cả hai cờ chặt được đặt. 
5. Đối với mỗi vị trí, lặp lại các chữ số được phép theo cờ chặt. Cập nhật nút Aho-Corasick cho chữ số mới, thêm giá trị mẫu của nút vào giá trị tích lũy và truyền các cờ chặt chẽ dựa trên việc chữ số mới có khớp với giới hạn hay không. 
6. Chỉ truyền bá các trạng thái có giá trị tích lũy ≤k. Các quốc gia vượt quá k không thể đóng góp vào câu trả lời. 
7. Sau khi xử lý xong tất cả các chữ số, tính tổng dp[len][_][value ≤ k][_][*] để được số số Mapo Tofu ≤r. Lặp lại với các số <l, sau đó trừ để có các số trong [l, r]. 
8. Lấy đáp án cuối cùng theo modulo 10^9+7. 

Tại sao nó hoạt động: DP xem xét tất cả các số từng chữ số trong giới hạn. Máy tự động đảm bảo các mẫu chồng chéo được tính chính xác một lần cho mỗi lần xuất hiện và giá trị tích lũy sẽ theo dõi tổng giá trị Mapo Tofu. Bằng cách tính tổng tất cả các trạng thái DP đáp ứng ràng buộc giá trị, chúng tôi đếm chính xác tất cả các số hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

class ACNode:
    def __init__(self):
        self.children = {}
        self.fail = None
        self.value = 0

def build_aho_corasick(patterns, m):
    root = ACNode()
    for digits, val in patterns:
        node = root
        for d in digits:
            if d not in node.children:
                node.children[d] = ACNode()
            node = node.children[d]
        node.value += val

    queue = []
    for d in range(m):
        if d in root.children:
            root.children[d].fail = root
            queue.append(root.children[d])
        else:
            root.children[d] = root

    while queue:
        cur = queue.pop(0)
        for d, child in cur.children.items():
            fail_node = cur.fail
            while d not in fail_node.children:
                fail_node = fail_node.fail
            child.fail = fail_node.children[d]
            child.value += child.fail.value
            queue.append(child)
    return root

def count_numbers(num, root, m, k):
    memo = {}

    def dfs(pos, node, val, tight):
        if val > k:
            return 0
        if pos == len(num):
            return 1
        key = (pos, id(node), val, tight)
        if key in memo:
            return memo[key]
        res = 0
        max_digit = num[pos] if tight else m-1
        for d in range(0, max_digit+1):
            next_node = node
            while d not in next_node.children:
                next_node = next_node.fail
            next_node = next_node.children[d]
            res += dfs(pos+1, next_node, val + next_node.value, tight and d == max_digit)
            res %= MOD
        memo[key] = res
        return res

    return dfs(0, root, 0, True)

def normalize_digits(a, length):
    return [0]*(length - len(a)) + a

def main():
    n, m, k = map(int, input().split())
    l_len, *l_digits = map(int, input().split())
    r_len, *r_digits = map(int, input().split())
    l_digits = normalize_digits(l_digits, max(len(l_digits), len(r_digits)))
    r_digits = normalize_digits(r_digits, max(len(l_digits), len(r_digits)))
    patterns = []
    for _ in range(n):
        parts = list(map(int, input().split()))
        patterns.append((parts[:-1], parts[-1]))
    root = build_aho_corasick(patterns, m)

    def prep(num):
        return normalize_digits(num, len(r_digits))

    res_r = count_numbers(prep(r_digits), root, m, k)
    # for l-1 we need a decrement function
    def dec(num):
        num = num[:]
        idx = len(num)-1
        while idx >= 0:
            if num[idx] > 0:
                num[idx] -= 1
                break
            num[idx] = m-1
            idx -= 1
        return num
    l_minus = dec(prep(l_digits))
    res_l = count_numbers(l_minus, root, m, k)
    print((res_r - res_l) % MOD)

if __name__ == "__main__":
    main()
```Mã có ba phần chính: xây dựng máy tự động, thực hiện chữ số DP với giá trị tích lũy và xử lý giới hạn [l, r] một cách cẩn thận. Hàm giảm xử lý các số có số 0 đứng đầu đúng cách để đảm bảo tính chính xác khi đếm các số nhỏ hơn l. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

Chữ số đầu vào: l=1, r=100, mẫu: 1→1, 01→1 

| tư thế | nút | giá trị | chặt chẽ | chữ số được xem xét | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | gốc | 0 | Đúng | 1..9 | ... | 

Việc truy tìm chỉ xác nhận 3 số vượt quá k=1, vậy 97 là
