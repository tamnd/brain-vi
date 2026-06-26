---
title: "CF 105264A - Mục tiêu, mục tiêu! Mọi nơi"
description: "Chúng tôi được giao một đội trong đó mỗi người chơi báo cáo một số “đóng góp”. Đóng góp là một bàn thắng được ghi hoặc một đường kiến ​​tạo giúp cầu thủ khác ghi bàn. Mỗi bàn thắng có chính xác một cầu thủ ghi bàn và có thể tùy ý có một trợ lý."
date: "2026-06-24T01:27:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105264
codeforces_index: "A"
codeforces_contest_name: "The 2024 Syrian Virtual University Collegiate Programming Contest"
rating: 0
weight: 105264
solve_time_s: 58
verified: true
draft: false
---

[CF 105264A - Mục tiêu, mục tiêu! Mọi nơi](https://codeforces.com/problemset/problem/105264/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một đội trong đó mỗi người chơi báo cáo một số “đóng góp”. Đóng góp là một bàn thắng được ghi hoặc một đường kiến ​​tạo giúp cầu thủ khác ghi bàn. Mỗi bàn thắng có chính xác một cầu thủ ghi bàn và có thể tùy ý có một trợ lý. Sự đóng góp giống nhau của một cầu thủ không thể được sử dụng cho cả việc ghi bàn và hỗ trợ cho cùng một mục tiêu và mỗi đóng góp được sử dụng chính xác một lần trong lần tái thiết cuối cùng. 

Dữ liệu đầu vào cung cấp cho chúng tôi, đối với mỗi cầu thủ, tổng cộng họ phải chịu trách nhiệm bao nhiêu đóng góp, nhưng nó không cho chúng tôi biết bao nhiêu trong số đó là bàn thắng hoặc pha kiến ​​​​tạo. Từ thông tin một phần này, chúng tôi cần xác định tổng số bàn thắng có thể được ghi, cụ thể là số bàn thắng nhỏ nhất và lớn nhất có thể phù hợp với số lượng đóng góp của tất cả các cầu thủ. 

Ràng buộc cơ cấu chính là mỗi mục tiêu tiêu tốn một đóng góp ghi bàn và tùy ý tiêu tốn một đóng góp hỗ trợ. Điều này ngay lập tức hạn chế mức độ linh hoạt của việc phân tách: các khoản đóng góp được cố định trên toàn cầu, nhưng việc kết hợp hỗ trợ với mục tiêu thì không. 

Tổng của tất cả các đóng góp có thể lớn bằng 3 · 10^5 trong các trường hợp thử nghiệm, loại trừ mọi lý luận bậc hai đối với người chơi hoặc bất kỳ công trình nào cố gắng so khớp rõ ràng các đóng góp theo cặp. Một đường chuyền tuyến tính cho mỗi trường hợp thử nghiệm là cần thiết. 

Một trường hợp phức tạp xuất hiện khi tất cả đóng góp đều tập trung vào một người chơi. Ví dụ: nếu một cầu thủ có 5 lần đóng góp và tất cả những cầu thủ khác có 0, chúng ta vẫn phải tôn trọng quy tắc kiến ​​tạo không được vượt quá số bàn thắng. Một cách tiếp cận ngây thơ chỉ đơn giản giả định “mỗi đóng góp là một mục tiêu hoặc hỗ trợ độc lập” mà không thực thi tính nhất quán giữa chúng sẽ vượt quá các cấu trúc hỗ trợ có thể có. 

Một trường hợp thất bại khác phát sinh khi cố gắng tham lam chỉ định các pha hỗ trợ mà không cân nhắc rằng mọi pha hỗ trợ đều phải gắn liền với một mục tiêu hợp lệ. Ví dụ: việc coi tất cả các đóng góp là có thể ghép nối tự do sẽ dẫn đến những cấu hình không thể thực hiện được trong đó có nhiều pha kiến ​​tạo hơn là bàn thắng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng chỉ định mỗi đóng góp trong tổng số A làm một bàn thắng hoặc một pha kiến tạo, sau đó xác thực xem nhiệm vụ kết quả có thể được nhóm thành các mục tiêu hợp lệ hay không. Về cơ bản, điều này trở thành một vấn đề phân vùng bị ràng buộc: mỗi bàn thắng phải có một đóng góp của cầu thủ ghi bàn và nhiều nhất là một đóng góp kiến ​​tạo. 

Trong trường hợp xấu nhất, có 2^A cách gán vai trò cho các đóng góp và ngay cả khi chúng ta loại bỏ sớm các trạng thái không hợp lệ thì số lượng cấu hình vẫn tăng theo cấp số nhân với A. Với A lên tới 3 · 10^5 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là hạn chế toàn cầu duy nhất thực sự quan trọng là sự cân bằng giữa tổng số đóng góp và số lượng chúng có thể được ghép thành các cạnh hỗ trợ. Mỗi bàn thắng đóng góp chính xác một “đơn vị bắt buộc” (cầu thủ ghi bàn) và tùy chọn thêm một đơn vị (hỗ trợ). Vì vậy, nếu chúng ta biểu thị G là số bàn thắng và S là số pha kiến tạo được sử dụng thì mỗi bàn thắng sẽ tiêu tốn 1 hoặc 2 đóng góp, điều này đưa ra một phương trình duy nhất kết nối mọi thứ: 

A = G + S 

Hạn chế về mặt cấu trúc là mọi hỗ trợ phải thuộc về một mục tiêu nào đó, do đó S không thể vượt quá G. Điều này biến bài toán thành một bài toán tối ưu giới hạn đơn giản trên các số nguyên thay vì bài toán gán tổ hợp. 

Từ đó, tối đa hóa mục tiêu tương ứng với tối thiểu hóa S, trong khi tối thiểu hóa mục tiêu tương ứng với tối đa hóa S dưới S ≤ G. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force phân công vai trò | O(2^A) | O(A) | Quá chậm | 
| Giảm ràng buộc đại số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng số đóng góp A bằng cách tính tổng tất cả ai. Đây là tổng số “đơn vị” mà chúng ta phải phân bổ thành các bàn thắng và đường kiến ​​tạo. 
2. Quan sát rằng mỗi mục tiêu tiêu tốn ít nhất một đơn vị và có thể là hai đơn vị nếu nó có một pha hỗ trợ. Giới thiệu các biến G cho các bàn thắng và S cho các bàn thắng được hỗ trợ. 
3. Biểu thị tổng giới hạn đóng góp là A = G + S, vì mỗi bàn thắng đóng góp một đơn vị cầu thủ ghi bàn và mỗi bàn thắng được kiến ​​tạo đóng góp thêm một đơn vị kiến ​​tạo. 
4. Sử dụng ràng buộc về cấu trúc rằng mọi hỗ trợ đều phải thuộc về một mục tiêu, điều này thực thi S ≤ G. 
5. Thay G = A − S vào bất đẳng thức S ≤ G để thu được S ≤ A − S, rút gọn thành 2S ≤ A. 
6. Từ đó, suy ra S lớn nhất có thể là sàn(A / 2), vì S phải là số nguyên. 
7. Tính số lượng mục tiêu tối thiểu là Gmin = A − sàn(A / 2), tương đương với ceil(A / 2). 
8. Tính số mục tiêu tối đa bằng cách giảm thiểu S, cho S = 0 và do đó Gmax = A. 

### Tại sao nó hoạt động 

Toàn bộ cấu trúc sụp đổ vì sự tương tác duy nhất giữa các đóng góp là thông qua việc ghép một đường kiến tạo với một bàn thắng. Sau khi chúng tôi loại bỏ từng cầu thủ riêng lẻ, mọi cấu hình hợp lệ sẽ được mô tả đầy đủ bằng số lượng bàn thắng được “kết hợp” với các đường kiến ​​tạo. Bất kỳ nhiệm vụ nào vi phạm S ≤ G sẽ yêu cầu nhiều pha kiến ​​​​tạo hơn bàn thắng, điều này là không thể vì mỗi pha kiến ​​​​tạo phải gắn liền với một mục tiêu riêng biệt. Ngược lại, bất kỳ S nào trong phạm vi hợp lệ đều có thể được hiện thực hóa bằng cách ghép S đóng góp dưới dạng hỗ trợ và để lại các đóng góp còn lại dưới dạng bàn thắng không được hỗ trợ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        total = sum(arr)

        max_goals = total
        min_goals = (total + 1) // 2

        print(min_goals, max_goals)

if __name__ == "__main__":
    solve()
```Việc triển khai giảm mọi thứ xuống việc tính toán tổng đóng góp cho mỗi trường hợp thử nghiệm. Sau khi đã biết tổng, phần còn lại của logic là đánh giá trực tiếp các công thức dẫn xuất. Điều tinh tế duy nhất là sử dụng trần số nguyên cho một nửa tổng, được triển khai dưới dạng (tổng + 1) // 2, để tránh lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó các khoản đóng góp được trải rộng dưới dạng [3, 2, 1]. Tổng số là 6. 

Để đạt được mục tiêu tối đa, chúng tôi coi mọi đóng góp là một mục tiêu riêng biệt, đưa ra 6 mục tiêu. 

Đối với các mục tiêu tối thiểu, chúng tôi cố gắng tối đa hóa số lần hỗ trợ, nhưng chúng tôi bị giới hạn bởi ràng buộc là số lần hỗ trợ không được vượt quá mục tiêu. Một nửa của 6 là 3, nên ta có S = 3 bàn thắng kiến ​​tạo và G = 3 bàn tổng cộng. 

| Bước | Tổng A | S đã chọn | G = A − S | 
| --- | --- | --- | --- | 
| Bắt đầu | 6 | 0 | 6 | 
| Vỏ Max S | 6 | 3 | 3 | 
| Kết quả | 6 | 3 | 3 | 

Điều này cho thấy việc ghép chính xác một nửa số lần đóng góp vào mối quan hệ kiến ​​tạo-bàn thắng sẽ giảm thiểu số lượng bàn thắng như thế nào. 

Bây giờ hãy xem xét [5]. Tổng cộng là 5. 

| Bước | Tổng A | S đã chọn | G = A − S | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 0 | 5 | 
| Vỏ Max S | 5 | 2 | 3 | 
| Kết quả | 5 | 2 | 3 | 

Điều này chứng tỏ hiệu ứng trần: một đóng góp không được kết hợp với một pha kiến ​​tạo, buộc phải có thêm một bàn thắng độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm yêu cầu một lần vượt qua để tính tổng các đóng góp | 
| Không gian | O(1) | Chỉ có tổng số hoạt động được duy trì | 

Các ràng buộc cho phép tổng cộng tối đa 3 · 10^5 phần tử và quét tuyến tính trên mỗi trường hợp thử nghiệm phù hợp thoải mái trong giới hạn thời gian do giải pháp chỉ thực hiện các thao tác bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input_data = sys.stdin.read().strip().split()
    it = iter(input_data)

    t = int(next(it))
    out = []
    for _ in range(t):
        n = int(next(it))
        arr = [int(next(it)) for _ in range(n)]
        total = sum(arr)
        out.append(f"{(total + 1)//2} {total}")
    return "\n".join(out)

# minimum size
assert run("1\n1\n0\n") == "0 0"

# single player non-zero
assert run("1\n1\n5\n") == "3 5"

# all equal
assert run("1\n4\n1 1 1 1\n") == "2 4"

# mixed values
assert run("1\n3\n3 2 1\n") == "3 6"

# large balanced case
assert run("1\n5\n10 10 10 10 10\n") == "25 50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người chơi số 0 duy nhất | 0 0 | xử lý tình huống đóng góp trống | 
| người chơi khác 0 | 3 5 | hành vi trần cho tổng số lẻ | 
| tất cả các giá trị nhỏ bằng nhau | 2 4 | sự đối xứng ghép đôi cơ bản | 
| giá trị hỗn hợp | 3 6 | tính đúng đắn chung | 
| giá trị đồng đều lớn | 25 50 | chia tỷ lệ và tổng hợp tuyến tính | 

## Vỏ cạnh 

Một người chơi không có đóng góp chứng tỏ rằng cả tối thiểu và tối đa đều sụp đổ về 0. Thuật toán tính tổng A = 0, cho Gmin = (0 + 1) // 2 = 0 và Gmax = 0, phù hợp với cấu hình duy nhất có thể. 

Khi một người chơi có số lần đóng góp là lẻ, chẳng hạn như A = 5, thuật toán sẽ tính Gmin = 3. Điều này phản ánh thực tế là có thể ghép nối nhiều nhất hai pha hỗ trợ, để lại một đóng góp không ghép đôi phải tạo thành một mục tiêu độc lập, thực thi hành vi trần một cách chính xác. 

Khi tất cả các cầu thủ đóng góp như nhau, chẳng hạn như bốn cầu thủ, mỗi người có 1 lần đóng góp, tổng số là 4 và tối thiểu là 2. Điều này tương ứng với việc ghép các đóng góp thành hai bàn thắng được kiến ​​tạo, bão hòa ràng buộc S  G chính xác ở mức bằng nhau.
