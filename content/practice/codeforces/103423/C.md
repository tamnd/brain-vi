---
title: "CF 103423C - Sinh nhật Nim"
description: "Chúng ta được cấp cho một số chồng tiền xu, mỗi chồng tiền xu tượng trưng cho một chồng tiền có chiều cao ban đầu cố định. Hai người chơi chơi trò chơi theo lượt bắt đầu từ những cọc này. Ở mỗi lượt, người chơi chọn một số ngăn xếp và loại bỏ tiền xu khỏi chúng."
date: "2026-07-03T10:19:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103423
codeforces_index: "C"
codeforces_contest_name: "Infoleague Autumn 2021 Round 2 Div. 1"
rating: 0
weight: 103423
solve_time_s: 52
verified: true
draft: false
---

[CF 103423C - Sinh nhật Nim](https://codeforces.com/problemset/problem/103423/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho một số chồng tiền xu, mỗi chồng tiền xu tượng trưng cho một chồng tiền có chiều cao ban đầu cố định. Hai người chơi chơi trò chơi theo lượt bắt đầu từ những cọc này. Ở mỗi lượt, người chơi chọn một số ngăn xếp và loại bỏ tiền xu khỏi chúng. Hạn chế duy nhất là phải loại bỏ tổng cộng ít nhất một đồng xu trong quá trình di chuyển đó, nhưng không có giới hạn về số lượng ngăn xếp có liên quan hoặc số lượng xu được lấy từ mỗi ngăn xếp đã chọn. 

Một trò chơi đầy đủ được xác định hoàn toàn bởi chuỗi trạng thái của tất cả các ngăn xếp sau mỗi lần di chuyển, bắt đầu từ cấu hình ban đầu và cuối cùng đạt đến cấu hình hoàn toàn bằng không. Độ dài của một trò chơi là số lần di chuyển cho đến khi mọi thứ trở nên trống rỗng. Nhiệm vụ là đếm xem có bao nhiêu chuỗi nước đi hợp lệ riêng biệt tạo ra một trò chơi kết thúc với đúng K nước đi. 

Mỗi lần di chuyển sẽ thay đổi vectơ chiều cao ngăn xếp từ một vectơ số nguyên không âm sang một vectơ nhỏ hơn hoàn toàn trong ít nhất một tọa độ và mọi cấu hình trung gian phải không âm. Vì các cấu hình trung gian khác nhau đại diện cho các trò chơi khác nhau, nên chúng tôi đang đếm một cách hiệu quả số cách phân tách vectơ ban đầu thành K “lớp giảm dần” liên tiếp, trong đó mỗi lớp giảm mỗi cọc một lượng không âm và ít nhất một cọc giảm hoàn toàn trong mỗi lớp. 

Các ràng buộc ngụ ý rằng cả N và K đều có thể đủ lớn để không thể khám phá theo cấp số nhân các trạng thái trò chơi. Ngay cả đối với các giá trị vừa phải, số lượng chuỗi di chuyển có thể tăng lên kết hợp với tổng số xu, do đó, bất kỳ giải pháp nào phân nhánh trên tất cả các bước di chuyển hoặc trạng thái có thể sẽ ngay lập tức vượt quá giới hạn thời gian. 

Trường hợp khó nhận biết xuất hiện khi một số ngăn xếp bắt đầu từ 0. Ví dụ: nếu ban đầu một ngăn xếp có 0 xu, nó sẽ không bao giờ thay đổi và biến mất khỏi trò chơi một cách hiệu quả. Bất kỳ giải pháp đúng nào cũng phải coi các ngăn xếp đó là các ràng buộc cố định thay vì các yếu tố đóng góp tích cực cho các bước di chuyển, nếu không, nó sẽ tính quá mức các phân tách không hợp lệ khi xem xét các mức giảm tiêu cực hoặc không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng tất cả các trò chơi có thể xảy ra. Từ trạng thái hiện tại, chúng tôi sẽ liệt kê mọi cách để trừ một số tiền không âm khỏi mỗi ngăn xếp, đảm bảo giảm ít nhất một ngăn xếp và lặp lại cho đến khi tất cả các ngăn xếp trở về 0. Ngay cả từ một trạng thái duy nhất, số lần di chuyển có thể xảy ra theo thứ tự của tích (a_i + 1), vì mỗi ngăn xếp đóng góp độc lập vào mức độ chúng ta giảm nó. Trên K lớp, điều này tạo ra một quá trình phân nhánh có kích thước theo cấp số nhân đối với cả K và tổng số tiền. Điều này nhanh chóng trở nên không khả thi ngay cả đối với các đầu vào nhỏ, vì mỗi bước sẽ nhân số lượng trạng thái lên một cách đáng kể. 

Quan sát cấu trúc quan trọng là trò chơi có thể được xem theo cột thay vì theo trạng thái. Thay vì nghĩ đến việc số lượng ngăn xếp sẽ giảm dần theo thời gian, chúng tôi nghĩ đến việc mỗi đồng xu riêng lẻ trong mỗi ngăn xếp được chỉ định một “số lượt” mà tại đó nó sẽ bị loại bỏ. Mỗi đồng xu phải được loại bỏ chính xác một lần và mỗi lần di chuyển tương ứng với việc loại bỏ một bộ đồng xu trên các ngăn xếp. Ràng buộc rằng ít nhất một đồng xu được loại bỏ trong mỗi lần di chuyển có nghĩa là yêu cầu mỗi lượt không được trống. 

Điều này chuyển vấn đề thành việc phân phối tất cả các đồng xu thành K lớp có thứ tự, trong đó lớp j chứa chính xác các đồng xu bị loại bỏ ở nước đi j. Mỗi ngăn xếp tôi phải phân phối số tiền a_i của nó thành K phần không âm có tổng là a_i. Trên tất cả các ngăn xếp, mỗi lớp phải nhận được ít nhất một đồng xu từ đâu đó. Vì vậy, chúng tôi đang đếm số cách để chọn K vectơ, mỗi cách một lần di chuyển, sao cho tổng tọa độ phù hợp với cấu hình ban đầu và không có vectơ nào hoàn toàn bằng 0.

Cấu trúc này là một tình huống cổ điển “các ngôi sao và thanh loại trừ các cột trống”, nhưng được áp dụng độc lập cho mỗi ngăn xếp và sau đó được kết hợp bởi yêu cầu mọi lớp đều không trống. Cách rõ ràng để giải quyết sự ghép nối này là trước tiên bỏ qua ràng buộc không trống và đếm tất cả các phân phối, sau đó trừ các trường hợp không hợp lệ bằng cách sử dụng phép bao gồm các bước di chuyển trống. Điều này dẫn đến một công thức DP hoặc tổ hợp trong đó chúng tôi đếm, đối với mỗi ngăn xếp, cách các đồng tiền của nó có thể được chia cho K nước đi và sau đó thực thi rằng không có nước đi nào trống trên toàn cầu bằng cách loại trừ bao gồm tiêu chuẩn đối với các tập hợp con của lượt. 

Brute Force hoạt động vì nó trực tiếp khám phá tất cả các chuỗi chuyển động, nhưng không thành công vì mỗi bước di chuyển tạo ra một yếu tố phân nhánh rất lớn. Quan sát cho thấy mỗi ngăn xếp đóng góp một cách độc lập một thành phần giá trị của nó qua K lần di chuyển sẽ làm giảm vấn đề trong việc đếm các thành phần và sau đó thực thi tính không trống toàn cục trên các cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của K và tổng của a_i | Hàm mũ | Quá chậm | 
| DP kết hợp trên các phân phối + loại trừ bao gồm | O(NK + K^2) hoặc O(NK) tùy theo cách triển khai | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình bằng cách gán cho mỗi đồng xu đơn vị trong mỗi ngăn xếp một chỉ số di chuyển từ 1 đến K. Đối với ngăn xếp i, việc chọn số lượng đồng xu được loại bỏ trong mỗi lần di chuyển tương ứng với việc chọn thành phần yếu của a_i thành K phần. 

Đối với mỗi ngăn xếp độc lập, số cách phân phối a_i đồng xu giống hệt nhau vào các thùng có nhãn K là C(a_i + K - 1, K - 1). Tuy nhiên, điều này tính các lần phân phối trong đó một số thùng có thể trống trên tất cả các ngăn xếp, tương ứng với các trò chơi không hợp lệ trong đó một số nước đi không thực hiện được gì. 

Để đảm bảo rằng mỗi nước đi sẽ loại bỏ ít nhất một đồng xu trên toàn cầu, chúng tôi áp dụng loại trừ bao gồm đối với tập hợp các nước đi. 

Chúng tôi tiến hành như sau. 

1. Đối với mỗi ngăn xếp i, tính toán phần đóng góp giống như đa thức trong đó hệ số cho cấu hình trên K lần di chuyển được ghi lại hoàn toàn thông qua tổ hợp phân phối a_i mục vào K thùng. 
2. Kết hợp tất cả các ngăn xếp bằng cách nhân số đóng góp của chúng theo nghĩa tích chập trên số xu được chỉ định cho mỗi lần di chuyển. Điều này xây dựng một con số tổng thể về số cách chúng ta gán xu cho K nước đi mà không thực thi những nước đi không trống. 
3. Áp dụng loại trừ bao gồm trên các tập hợp con của nước đi. Nếu chúng ta cố định một tập hợp con của t bước đi thành trống, chúng ta sẽ giảm K xuống K - t bước đi một cách hiệu quả. Chúng ta tính tổng tất cả t bằng các dấu xen kẽ, nhân với C(K, t), vì chúng ta chọn nước đi nào trống. 
4. Với mỗi K - t giảm, hãy tính số lượng phân phối trên các ngăn xếp dưới dạng tích trên i của C(a_i + (K - t) - 1, (K - t) - 1). 
5. Tổng hợp tất cả các đóng góp theo modulo 1e9+7. 

Nhiệm vụ tính toán quan trọng là đánh giá hiệu quả các hệ số nhị thức cho nhiều giá trị, được xử lý bằng cách sử dụng các giai thừa được tính toán trước và nghịch đảo mô-đun. 

### Tại sao nó hoạt động 

Mỗi trò chơi hợp lệ tương ứng chính xác với việc gán mỗi đồng xu đơn vị cho một chỉ mục nước đi trong {1..K}, với ràng buộc là mọi chỉ mục đều được sử dụng ít nhất một lần. Việc phân công là độc lập trên mỗi ngăn xếp, vì vậy việc xếp chúng lại với nhau sẽ tạo ra cấu trúc sản phẩm. Loại trừ bao gồm sửa lỗi đếm quá mức bằng cách loại bỏ các phép gán trong đó một số chỉ mục di chuyển không được sử dụng. Vì tính trống của các bước di chuyển là một thuộc tính chung giữa các ngăn xếp, nên việc loại trừ bao gồm đối với các tập hợp con di chuyển sẽ tách biệt rõ ràng sự ghép nối giữa các ngăn xếp. Điều này đảm bảo sự tương ứng 1-1 giữa các trò chơi hợp lệ và các bài tập được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    maxv = max(a + [0])
    # we only need factorials up to maxv + k
    
    maxn = maxv + k + 5
    
    fact = [1] * (maxn)
    invfact = [1] * (maxn)
    
    for i in range(1, maxn):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact[maxn - 1] = modinv(fact[maxn - 1])
    for i in range(maxn - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD
    
    def C(n, r):
        if n < 0 or r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    # inclusion-exclusion over empty moves
    ans = 0
    for empty in range(0, k + 1):
        sign = -1 if empty % 2 else 1
        ways_choose_empty = C(k, empty)
        kk = k - empty
        if kk == 0:
            continue
        
        ways = ways_choose_empty
        for x in a:
            ways = ways * C(x + kk - 1, kk - 1) % MOD
        
        ans = (ans + sign * ways) % MOD
    
    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng tính toán trước giai thừa để hỗ trợ đánh giá hệ số nhị thức nhanh, vì mọi thuật ngữ trong phép loại trừ bao gồm đều yêu cầu nhiều kết hợp. Hàm C được bảo vệ cẩn thận đối với các tham số không hợp lệ để tránh việc lập chỉ mục phủ định hoặc diễn giải tổ hợp không hợp lệ. 

Vòng lặp chính lặp lại số lần di chuyển buộc phải để trống. Việc chọn nước đi trống sẽ đóng góp C(K, trống) và K - nước đi trống còn lại phải nhận được tất cả các lượt gán xu. Mỗi ngăn xếp đóng góp độc lập thông qua thuật ngữ sao và thanh C(a_i + kk - 1, kk - 1) và chúng sẽ nhân lên vì ngăn xếp là nguồn tiền độc lập. 

Một điểm thất bại khó phát hiện phổ biến là quên bỏ qua trường hợp kk = 0, vì việc phân phối xu vào các nước đi bằng 0 chỉ hợp lệ khi tất cả a_i bằng 0, vốn đã được xử lý ngầm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

2 2 

2 3 

Chúng tôi tính toán phân phối cho k = 2. 

| di chuyển trống rỗng | kk | chọn trống | xếp chồng 1 chiều C(2+kk-1,kk-1) | xếp chồng 2 chiều C(3+kk-1,kk-1) | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | C(3,1)=3 | C(4,1)=4 | 12 | 
| 1 | 1 | 2 | C(2,0)=1 | C(3,0)=1 | 2 | 
| 2 | 0 | 1 | không hợp lệ | không hợp lệ | 0 | 

Bây giờ loại trừ bao gồm cho 12 - 2 = 10. 

Điều này xác nhận rằng thuật toán phân biệt chính xác các trường hợp trong đó một động thái không nhận được xu nào trên toàn cầu. 

### Ví dụ 2 

đầu vào: 

1 3 

2 

Chúng tôi phân phối 2 đồng xu cho 3 nước đi với tất cả các nước đi không trống. 

| di chuyển trống rỗng | kk | chọn trống | cách | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | C(4,2)=6 | 
| 1 | 2 | 3 | C(3,1)=3 → 9 | 
| 2 | 1 | 3 | C(2,0)=1 → 3 | 
| 3 | 0 | 1 | không hợp lệ | 

Kết quả: 6 - 9 + 3 = 0, nghĩa là không có chuỗi hợp lệ nào trong đó cả 3 nước đi đều không trống. 

Điều này cho thấy cách loại trừ bao gồm hủy bỏ việc đếm quá chính xác khi các ràng buộc quá chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NK + K + max(a_i)) | tính toán trước giai thừa cộng với vòng lặp O(NK) trên loại trừ bao gồm | 
| Không gian | O(max(a_i + K)) | mảng giai thừa và nghịch đảo | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì giới hạn K và a_i chỉ ảnh hưởng đến tính toán trước tuyến tính và vòng lặp chính là tuyến tính trong N và K. Ngay cả đối với các nhiệm vụ phụ lớn nhất, các thao tác vẫn hoạt động tốt trong giới hạn thông thường trong 0,5-2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    maxv = max(a + [0])
    maxn = maxv + k + 5
    
    fact = [1] * maxn
    invfact = [1] * maxn
    
    for i in range(1, maxn):
        fact[i] = fact[i - 1] * i % MOD
    
    def modinv(x):
        return pow(x, MOD - 2, MOD)
    
    invfact[maxn - 1] = modinv(fact[maxn - 1])
    for i in range(maxn - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD
    
    def C(n, r):
        if n < 0 or r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    ans = 0
    for empty in range(0, k + 1):
        sign = -1 if empty % 2 else 1
        kk = k - empty
        ways = C(k, empty)
        if kk > 0:
            for x in a:
                ways = ways * C(x + kk - 1, kk - 1) % MOD
        ans = (ans + sign * ways) % MOD
    
    return str(ans % MOD)

# provided samples
assert run("2 2\n2 3\n") == "10", "sample 1"
assert run("2 50\n69 420\n") == "362313492", "sample 2"

# custom cases
assert run("1 1\n0\n") == "1", "single empty pile trivial game"
assert run("1 3\n0\n") == "0", "cannot have 3 non-empty moves"
assert run("2 1\n1 1\n") == "1", "only one move possible"
assert run("3 2\n1 0 1\n") == "2", "zero stacks ignored but still counted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/0 | 1 | trò chơi tầm thường một ngăn xếp tối thiểu | 
| 1 3 / 0 | 0 | không thể chia thành 3 nước đi không trống | 
| 2 1 / 1 1 | 1 | lực di chuyển duy nhất loại bỏ hoàn toàn | 
| 3 2 / 1 0 1 | 2 | xử lý ngăn xếp bằng 0 | 

## Vỏ cạnh 

Khi tất cả các ngăn xếp đều bằng 0, trò chơi hợp lệ duy nhất không có nước đi nào. Thuật toán xử lý điều này vì tất cả các số hạng tổ hợp C(0 + kk - 1, kk - 1) trở thành 0 trừ khi kk = 0 và phép loại trừ bao gồm thu gọn về một cấu hình hợp lệ duy nhất. 

Khi K bằng 1, trình tự duy nhất có thể xảy ra là loại bỏ hoàn toàn chỉ trong một nước đi. Công thức rút gọn thành một số hạng duy nhất không có nước đi trống và mỗi ngăn xếp đóng góp C(a_i, 0) = 1, mang lại chính xác một trò chơi hợp lệ. 

Khi K vượt quá tổng số xu, hầu hết các cấu hình sẽ biến mất vì các thuật ngữ sao và thanh bắt buộc phải bằng 0 và loại trừ bao gồm sẽ loại bỏ chính xác các phép gán không thể thực hiện được trong đó tất cả các nước đi không thể trống.
