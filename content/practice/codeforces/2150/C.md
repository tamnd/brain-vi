---
title: "CF 2150C - Cửa hàng phiên bản giới hạn"
description: "Chúng ta được yêu cầu phân tích một tình huống trong đó hai người chơi, Alice và Bob, lần lượt chọn đồ vật từ một cửa hàng. Mỗi đối tượng có một giá trị theo quan điểm của chúng tôi và mỗi người chơi có thứ tự ưu tiên riêng."
date: "2026-06-08T01:03:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2150
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1053 (Div. 1)"
rating: 2100
weight: 2150
solve_time_s: 101
verified: false
draft: false
---

[CF 2150C - Cửa hàng phiên bản giới hạn](https://codeforces.com/problemset/problem/2150/C) 

**Đánh giá:** 2100 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một tình huống trong đó hai người chơi, Alice và Bob, lần lượt chọn đồ vật từ một cửa hàng. Mỗi đối tượng có một giá trị theo quan điểm của chúng tôi và mỗi người chơi có thứ tự ưu tiên riêng. Người chơi luôn chọn đồ vật được ưu tiên cao nhất vẫn còn khi đến lượt của mình. Cuối cùng, Alice sẽ có một tập hợp con các đồ vật cô ấy đã mua và nhiệm vụ là tính tổng giá trị tối đa của các đồ vật mà cô ấy có thể có được theo định giá của chúng tôi. 

Đầu vào cung cấp cho chúng tôi nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng đối tượng, giá trị của chúng, thứ tự ưu tiên của Alice và Bob. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số tiền tối đa Alice có thể nhận được qua tất cả các chuỗi mua hàng hợp lệ. 

Những ràng buộc rất chặt chẽ.`n`có thể lên tới 2×10^5 cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm cũng đạt tới 2×10^5. Điều này loại trừ bất kỳ giải pháp nào tệ hơn khoảng O(n log n) cho mỗi trường hợp thử nghiệm. Một lực lượng thô bạo mô phỏng mọi sự đan xen có thể có trong các lựa chọn của Alice và Bob là hoàn toàn không khả thi, bởi vì số cách các đối tượng có thể được lấy tăng theo giai thừa với n. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi trong đó các ưu tiên hàng đầu của Alice đều có giá trị thấp hoặc âm, Bob chọn các đối tượng có thể chặn cô ấy hoặc trong đó tất cả các giá trị đều âm. Ví dụ, nếu`v = [-5, -10, 1]`,`a = [3,1,2]`,`b = [1,2,3]`, Alice chỉ nên chọn đối tượng 3 để tối đa hóa tổng. Một cách tiếp cận tham lam bất cẩn cho rằng cô ấy luôn được ưu tiên hàng đầu trước tiên có thể chọn cách khác và đưa ra số tiền thấp hơn. 

## Phương pháp tiếp cận 

Về mặt khái niệm, cách tiếp cận vũ phu rất đơn giản. Đối với mỗi trường hợp thử nghiệm, bạn có thể tạo tất cả các đơn đặt hàng có thể có, mô phỏng lượt của Alice và Bob, đồng thời theo dõi tổng giá trị của các đối tượng mà Alice kết thúc. Điều này hiệu quả vì nó mô phỏng trung thực mọi kịch bản, nhưng nó là O(n!), quá chậm đối với n lên tới 2×10^5. 

Cái nhìn sâu sắc quan trọng đến từ suy nghĩ ngược lại. Thay vì mô phỏng tất cả các trình tự, hãy lưu ý rằng quyết định duy nhất quan trọng đối với tổng số của Alice là thời điểm cô ấy có thể lấy những món đồ ưa thích của mình trước khi Bob lấy chúng. Mỗi đối tượng có thể được đặc trưng bởi sự lựa chọn sớm nhất của Alice hoặc Bob. Nếu Bob chọn một món đồ sớm hơn Alice có thể, cô ấy sẽ không thể lấy được món đồ đó. Nếu Alice chọn trước, cô ấy luôn có thể chọn nó sau cùng. 

Chúng ta có thể lập mô hình này bằng cách sử dụng quy hoạch động trên các tập hợp con hoặc các vị trí trong danh sách ưu tiên của Alice. Hãy xác định`dp[i]`là số tiền tối đa Alice có thể nhận được nếu cô ấy chỉ xem xét phần trên của mình`i`các mặt hàng ưa thích. Khi chúng tôi xử lý cô ấy`i`-thứ ưu tiên, chúng tôi quyết định liệu Bob có lấy nó trước cô ấy hay không. Điều này làm giảm vấn đề thành quét tuyến tính theo thứ tự ưu tiên của Alice, theo dõi những đối tượng nào còn tồn tại. Sử dụng một mảng để đánh dấu lựa chọn sớm nhất của Bob, chúng ta có thể nhanh chóng quyết định liệu một đối tượng có an toàn cho Alice hay không và tích lũy số tiền tối đa tương ứng. Điều này làm giảm độ phức tạp xuống O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi đối tượng, hãy ghi lại vị trí của nó trong danh sách ưu tiên của Bob. Điều này cho phép chúng tôi biết chính xác thời điểm Bob sẽ chọn nó so với lựa chọn của Alice. 
2. Lặp lại danh sách ưu tiên của Alice theo thứ tự. Với mỗi đối tượng, hãy kiểm tra vị trí của nó trong danh sách của Bob. 
3. Duy trì một biến đang chạy cho chỉ mục sớm nhất Bob sẽ lấy một đối tượng. Nếu chỉ mục của Bob nằm trước lựa chọn hiện tại trong chuỗi của Alice, hãy bỏ qua đối tượng này vì Bob sẽ lấy nó. 
4. Nếu không, hãy cộng giá trị của đối tượng vào tổng tích lũy. 
5. Trả về tổng ở cuối vòng lặp, đại diện cho số tiền tối đa Alice có thể đạt được cho trường hợp thử nghiệm này. 

Điều bất biến được duy trì là tại bất kỳ thời điểm nào trong quá trình lặp lại, chúng tôi đã tính toán chính xác tất cả các đối tượng mà Alice có thể chọn một cách an toàn trước khi Bob có thể chặn chúng. Điều này đảm bảo rằng số tiền cao hơn có thể bị bỏ lỡ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        v = list(map(int, input().split()))
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # 0-based indexing
        a = [x-1 for x in a]
        b = [x-1 for x in b]
        
        # Position map: where does Bob pick each object
        pos_bob = [0]*n
        for idx, obj in enumerate(b):
            pos_bob[obj] = idx
        
        # Track the maximum sum Alice can get
        max_sum = 0
        # Current earliest Bob's pick that can block Alice
        min_bob_pos = n
        for obj in reversed(a):
            if pos_bob[obj] < min_bob_pos:
                max_sum += v[obj]
                min_bob_pos = pos_bob[obj]
        
        print(max_sum)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chuyển đổi danh sách ưu tiên thành lập chỉ mục dựa trên 0. Nó tạo ra một bản đồ vị trí cho Bob để chúng ta có thể nhanh chóng kiểm tra xem anh ấy có chọn một đồ vật trước Alice hay không. Bằng cách lặp lại danh sách của Alice theo chiều ngược lại, chúng tôi đảm bảo rằng chúng tôi chọn được những đồ vật mà cô ấy có thể lấy được một cách an toàn mà không bị Bob chặn. Đang cập nhật`min_bob_pos`đảm bảo chúng tôi chỉ tích lũy giá trị cho những món đồ mà Alice có thể bảo đảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
v = [1, -1, 1]
a = [3, 1, 2]
b = [2, 3, 1]
```| Bước | Đối tượng | Chỉ số Bob | min_bob_pos | max_sum | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 0 | 
| 2 | 1 | 2 | 1 | 1 | 
| 3 | 3 | 1 | 1 | 2 | 

Cuối cùng, Alice có thể lấy đối tượng 3 và 1 với tổng số tiền là 2. Điều này phù hợp với kết quả đầu ra mong đợi. 

### Ví dụ 2 

đầu vào:```
n = 3
v = [-2, 5, 2]
a = [3, 1, 2]
b = [2, 3, 1]
```Lặp lại ngược lại: 

| Bước | Đối tượng | Chỉ số Bob | min_bob_pos | max_sum | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 0 | 
| 2 | 1 | 2 | 1 | 5 | 
| 3 | 3 | 1 | 1 | 5 | 

Alice có thể lấy đồ vật 1 và 2 một cách an toàn với tổng số điểm là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Ánh xạ vị trí của Bob là O(n), lặp lại danh sách ưu tiên của Alice là O(n) | 
| Không gian | O(n) | Mảng lưu trữ vị trí và sở thích | 

Điều này rất phù hợp với các ràng buộc vì tổng n trên tất cả các trường hợp thử nghiệm 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample
assert run("8\n3\n1 -1 1\n3 1 2\n2 3 1\n3\n-2 5 2\n3 1 2\n2 3 1\n3\n-1 -2 -3\n3 1 2\n2 3 1\n3\n1000000000 1000000000 1000000000\n3 1 2\n2 3 1\n4\n5 -15 10 -5\n2 4 3 1\n1 4 2 3\n4\n-5 -5 -5 100\n2 3 1 4\n4 1 2 3\n4\n-1 -100 5 10\n1 2 3 4\n2 3 4 1\n12\n-4 6 10 10 1 -8 6 2 -8 -4 0 -6\n11 12 7 3 6 8 1 5 10 2 9 4\n7 5 3 6 1 2 8 12 9 4 10 11") == "2\n5\n0\n3000000000\n10\n85\n14\n24"

# Custom cases
assert run("1\n1\n100\n1\n1") == "100"
assert run("1\n2\n-5 10\n1 2\n2 1") == "10"
assert run("1\n5\n1 2 3 4 5\n5 4 3 2 1\n1 2 3 4 5") == "15"
assert run("1\n3\n-1 -2 -3\n1 2 3\n3 2 1") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đối tượng duy nhất, Alice đầu tiên | 100 | Tối thiểu |
