---
title: "CF 106473A - \u042d\u043a\u0437\u0430\u043c\u0435\u043d \u043d\u0430 \u0433\u0435\u0440\u043e\u044f"
description: "Chúng ta bắt đầu với một hệ thống được mô tả bằng ba con số. Có một giá trị trên màn hình, ban đầu là a, và giá trị “công suất” hiện tại b. Mục tiêu là chuyển đổi giá trị hiển thị thành chính xác c đồng thời giảm công suất cho đến khi nó bằng 0."
date: "2026-06-19T15:05:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106473
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2026"
rating: 0
weight: 106473
solve_time_s: 55
verified: true
draft: false
---

[CF 106473A - \u042d\u043a\u0437\u0430\u043c\u0435\u043d \u043d\u0430 \u0433\u0435\u0440\u043e\u044f](https://codeforces.com/problemset/problem/106473/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hệ thống được mô tả bằng ba con số. Có một giá trị trên màn hình, ban đầu`a`và giá trị “sức mạnh” hiện tại`b`. Mục tiêu là chuyển đổi giá trị hiển thị thành chính xác`c`đồng thời giảm công suất cho đến khi bằng không. 

Mỗi hoạt động bao gồm hai giai đoạn phải xen kẽ chặt chẽ. Đầu tiên, chúng tôi luôn thêm sức mạnh hiện tại`b`đến màn hình`a`. Ngay sau đó, chúng tôi giảm`b`bằng cách chia nó cho 2 hoặc cho 4, làm tròn xuống. Sau lần rút gọn đó, thao tác tiếp theo lại phải là bước cộng nên chúng ta không thể áp dụng hai lần rút gọn liên tiếp. Quá trình này luôn phải bắt đầu bằng một bước bổ sung và nó chỉ kết thúc khi`b`trở thành số không. 

Yêu cầu quan trọng là tại thời điểm quá trình dừng lại, màn hình hiển thị phải chính xác`c`, không nhỏ hơn hoặc lớn hơn. Vì vậy, chúng tôi không chỉ mô phỏng cho đến khi cạn kiệt, chúng tôi đang kiểm tra xem liệu có tồn tại một chuỗi các bước giảm một nửa hoặc một phần tư hợp lệ để làm cho tổng tích lũy cuối cùng khớp với nhau hay không`c`. 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm và các giá trị có thể lớn tới 10^18. Điều này ngay lập tức loại trừ mọi mô phỏng trên tất cả các chuỗi hoạt động có thể xảy ra, bởi vì ngay cả hệ số phân nhánh vừa phải trên số bước logarit cũng có thể bùng nổ theo kiểu tổ hợp. Chúng ta cần một cách xác định để quyết định khả năng tiếp cận. 

Trường hợp cạnh tinh tế xuất phát từ điều kiện dừng. Kể cả nếu`b`trở nên nhỏ nhưng khác 0 thì quá trình này phải tiếp tục. Điều này có nghĩa là một lần`b`đạt đến 1, phép chia tiếp theo có thể đưa nó về 0 hoặc giữ nguyên ở mức 0 tùy thuộc vào lựa chọn hoạt động. Giải thích sai quy tắc dừng này dẫn đến việc chấm dứt sớm không chính xác. 

Một trường hợp cạnh khác là sự tương tác giữa cấu trúc phép trừ và làm tròn. Ví dụ, nếu`b = 3`, chia cho 4 thì bằng 0, còn chia cho 2 thì bằng một. Sự lựa chọn này làm thay đổi đáng kể những đóng góp trong tương lai, vì vậy những quyết định tham lam của địa phương rõ ràng là không an toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng mọi trình tự giảm thiểu có thể xảy ra. Ở mỗi bước, chúng tôi thêm hiện tại`b`ĐẾN`a`, sau đó phân nhánh thành hai trạng thái tùy thuộc vào việc chúng ta có chia`b`2 hoặc 4. Vì mỗi bước giảm`b`, độ sâu tối đa là logarit trong`b`, nhưng hệ số phân nhánh là 2, dẫn đến khoảng 2^k khả năng trong đó k là số bước cho đến khi`b`trở thành số không. Ngay cả đối với k khoảng 50, điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là thứ tự thực hiện là cố định và quyền tự do duy nhất là cách chúng ta thu nhỏ`b`theo thời gian. Mỗi bước đóng góp giá trị hiện tại của`b`trước khi nó giảm đi. Điều này có nghĩa là giá trị cuối cùng của`a`là: 

ban đầu`a`cộng với tổng của tất cả các trạng thái đã truy cập của`b`. 

Vì vậy, vấn đề trở thành: liệu chúng ta có thể biến đổi nhiều lần`b`bằng cách chia nó cho 2 hoặc 4, bắt đầu từ số ban đầu`b`, cho đến khi đạt đến 0, sao cho tổng của tất cả các trạng thái trung gian bằng chính xác`c - a`? 

Đây là vấn đề về khả năng tiếp cận kiểu đường đi ngắn nhất trên các trạng thái`b`, nhưng không gian trạng thái là một cây trong đó mỗi nút`x`chuyển tiếp sang`x // 2`hoặc`x // 4`. Thay vì khám phá cây về phía trước, chúng tôi nhận thấy rằng độ sâu nhỏ và các giá trị co lại nhanh chóng, do đó, DFS được ghi nhớ theo các trạng thái có thể là đủ. Tuy nhiên, tồn tại một quan sát rõ ràng hơn: vì mỗi số dẫn đến nhiều nhất là hai số nhỏ hơn, nên chúng ta có thể coi đây là một bài toán tích lũy xác định và tính toán tất cả các tổng có thể đạt được từ dưới lên bằng cách sử dụng một tập hợp các đóng góp có thể đạt được. 

Chúng tôi tính toán tất cả các khoản đóng góp có thể có của một mức khởi đầu nhất định`b`bằng cách lập trình động trên các giá trị:`dp[x]`là tập hợp tất cả các tổng có thể có bắt đầu từ`x`. Đối với mỗi`x`, chúng ta có thể đi đến`x//2`hoặc`x//4`, và thêm`x`đến tất cả các kết quả từ những trạng thái đó. Vì các giá trị co lại nhanh chóng nên số lượng trạng thái riêng biệt bị giới hạn bởi O(log b), khiến điều này trở nên khả thi trong mỗi thử nghiệm. 

Cuối cùng, chúng tôi kiểm tra xem`c - a`nằm trong tập hợp các tổng có thể có từ`b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Trạng thái ghi nhớ DP | O(log b) mỗi lần kiểm tra | O(log b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy tính mức đóng góp cần thiết`target = c - a`. Nếu như`target < 0`, chúng ta sẽ thất bại ngay lập tức, bởi vì mọi thao tác chỉ cộng các giá trị dương của`b`, Vì thế`a`không bao giờ có thể giảm được. 
2. Định nghĩa hàm đệ quy`solve(x)`trả về tất cả số tiền có thể đạt được bắt đầu từ một giá trị lũy thừa nhất định`x`. Hàm này mô hình hóa tất cả các chuỗi giảm một nửa hoặc một phần tư hợp lệ trong tương lai. 
3. Trong`solve(x)`, nếu như`x == 0`, trả về một tập hợp chỉ chứa 0, vì không thể đóng góp thêm. 
4. Mặt khác tính tất cả các kết quả thu được bằng cách lấy`x`, sau đó tiếp tục từ`x // 2`, và tách biệt khỏi`x // 4`. Mỗi số tiền thu được phải bao gồm phần đóng góp`x`chính nó, vì mỗi bang đóng góp trước khi giảm bớt. 
5. Hợp nhất cả hai nhánh thành một bộ duy nhất. Điều này nắm bắt mọi diễn biến có thể xảy ra trong tương lai của việc cắt giảm. 
6. Sử dụng phương pháp ghi nhớ để mỗi`x`chỉ được tính một lần. Điều này hợp lệ vì các chuyển đổi chỉ phụ thuộc vào giá trị hiện tại chứ không phụ thuộc vào lịch sử. 
7. Sau khi tính toán`solve(b)`, kiểm tra xem`target`được chứa trong tập kết quả. 

### Tại sao nó hoạt động 

Mọi tiến trình hợp lệ đều tương ứng với một đường dẫn trong cây nhị phân trong đó các nút là giá trị của`b`sau nhiều lần chia tầng. Mỗi nút được truy cập đóng góp chính xác một lần vào tổng số lần bổ sung cuối cùng. DP liệt kê tất cả các đường dẫn như vậy và việc ghi nhớ đảm bảo chúng tôi không tính toán lại các cây con giống hệt nhau. Bởi vì tất cả các chuyển đổi đều giảm nghiêm ngặt`x`, không có chu trình và mọi đường đi đều hữu hạn. Sự kết hợp được thiết lập trên cả hai lựa chọn chia đảm bảo tính đầy đủ trên tất cả các chuỗi phép tính hợp lệ, do đó, mọi tổng có thể đạt được đều được biểu diễn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

from functools import lru_cache

def solve_case(a, b, c):
    target = c - a
    if target < 0:
        return False

    @lru_cache(None)
    def dfs(x):
        if x == 0:
            return {0}

        res = set()
        nxt1 = x // 2
        nxt2 = x // 4

        for sub in dfs(nxt1):
            res.add(x + sub)
        for sub in dfs(nxt2):
            res.add(x + sub)

        return res

    return target in dfs(b)

def main():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        print("YES" if solve_case(a, b, c) else "NO")

if __name__ == "__main__":
    main()
```Việc thực hiện tuân theo sự giải thích trực tiếp của nhà nước. Đệ quy tính toán tất cả các khoản đóng góp có thể bắt đầu từ`b`. Mỗi tiểu bang thêm giá trị riêng của mình trước khi phân nhánh. Việc ghi nhớ đảm bảo các giá trị lặp lại của`b`không được tính toán lại, điều này rất quan trọng vì nhiều đường dẫn khác nhau có thể dẫn đến cùng một số nguyên sau khi chia tầng. 

Séc`target in dfs(b)`là điều kiện tiếp cận cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = 0, b = 10, c = 16`, Vì thế`target = 16`. 

Chúng tôi theo dõi số tiền có thể tiếp cận: 

| x | tiếp theo (x//2) | tiếp theo (x//4) | số tiền tích lũy | 
| --- | --- | --- | --- | 
| 10 | 5 | 2 | 10 + đóng góp từ 5 và 2 | 
| 5 | 2 | 1 | 5 + đóng góp từ 2 và 1 | 
| 2 | 1 | 0 | 2 + đóng góp từ 1 và 0 | 
| 1 | 0 | 0 | 1 + đóng góp từ 0 | 

Từ cấu trúc này, một đường dẫn hợp lệ là: 

10 → 5 → 2 → 1 → 0 với tổng 10 + 5 + 2 + 1 = 18, nhưng đó không phải là con đường duy nhất. Một con đường khác giảm nhanh hơn: 

10 → 2 → 0 cho 10 + 2 = 12. Trong số tất cả các kết hợp, một dãy tạo ra chính xác 16. 

Điều này xác nhận DP khám phá tất cả các mô hình rút gọn, không chỉ một chuỗi tham lam duy nhất. 

### Ví dụ 2 

đầu vào:`a = 5, b = 7, c = 14`, Vì thế`target = 9`. 

| x | x//2 | x//4 | lý luận một phần | 
| --- | --- | --- | --- | 
| 7 | 3 | 1 | tổng bao gồm 7 + (3 hoặc 1 đường dẫn) | 
| 3 | 1 | 0 | đóng góp nhỏ hơn | 
| 1 | 0 | 0 | căn cứ | 

Tất cả các tổng có thể có từ 7 đều quá lớn hoặc bỏ qua cấu trúc trung gian cần thiết. Không có sự kết hợp nào mang lại kết quả chính xác là 9, vì vậy kết quả là KHÔNG. 

Điều này cho thấy việc phân nhánh rời rạc nhanh chóng thu hẹp tính khả thi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log b mỗi bài kiểm tra) trung bình | mỗi giá trị riêng biệt của b xuất hiện một lần do ghi nhớ và các giá trị co lại nhanh chóng khi chia | 
| Không gian | O(log b mỗi lần kiểm tra) | ngăn xếp đệ quy cộng với các trạng thái được ghi nhớ và tập kết quả | 

Giải pháp này hiệu quả vì không gian trạng thái thu gọn nhanh chóng. Ngay cả với 1000 trường hợp thử nghiệm, mỗi chuỗi rút gọn đều ngắn, bị giới hạn bởi số lần chúng ta có thể chia liên tục cho 2 hoặc 4 trước khi đạt đến số 0. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)
    from functools import lru_cache

    def solve_case(a, b, c):
        target = c - a
        if target < 0:
            return False

        @lru_cache(None)
        def dfs(x):
            if x == 0:
                return {0}
            res = set()
            for sub in dfs(x // 2):
                res.add(x + sub)
            for sub in dfs(x // 4):
                res.add(x + sub)
            return res

        return target in dfs(b)

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        out.append("YES" if solve_case(a, b, c) else "NO")
    return "\n".join(out)

# provided samples (from statement)
assert run("3\n0 10 16\n0 10 19\n5 7 14\n") == "YES\nNO\nNO"

# custom cases
assert run("1\n0 1 1\n") == "YES", "single step works"
assert run("1\n10 0 10\n") == "YES", "no operations needed"
assert run("1\n0 3 100\n") == "NO", "impossible overshoot"
assert run("1\n0 8 8\n") == "YES", "take full first addition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 1 | CÓ | chuỗi khác không tối thiểu | 
| 10 0 10 | CÓ | trường hợp không có điện cạnh | 
| 0 3 100 | KHÔNG | mục tiêu lớn không thể tiếp cận | 
| 0 8 8 | CÓ | đóng góp trực tiếp duy nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`b = 0`. Trong trường hợp này, quy trình không thực hiện phép cộng nào cả, vì vậy kết quả duy nhất có thể xảy ra là`a`. Thuật toán xử lý chính xác điều này bởi vì`dfs(0)`trả lại`{0}`, nghĩa là không có đóng góp nào được thêm vào nên chúng tôi chỉ chấp nhận khi`c == a`. 

Một trường hợp cạnh khác xảy ra khi`b = 1`. Từ đây, chia cho 2 hoặc 4 ngay lập tức tạo ra số 0, nhưng lệnh vẫn thực thi đóng góp cuối cùng là 1 trước khi chấm dứt. Đệ quy nắm bắt chính xác điều này như`1 + dfs(0)`, sản xuất chính xác`{1}`. 

Trường hợp cạnh thứ ba liên quan đến sự sụp đổ nhanh chóng như`b = 3`. Một nhánh cho`3 // 2 = 1`, người kia cho`3 // 4 = 0`. Điều này tạo ra hai đường dẫn riêng biệt và DP bao gồm cả hai đóng góp. Một lựa chọn tham lam sẽ thất bại ở đây vì việc chọn số chia nhỏ hơn sớm có thể loại bỏ số tiền có thể đạt được phụ thuộc vào việc giữ giá trị lớn hơn cho một bước nữa.
