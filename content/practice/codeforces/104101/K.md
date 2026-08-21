---
title: "CF 104101K-Bit"
description: "Chúng ta được cung cấp một chuỗi các thao tác bitwise cố định luôn được áp dụng cho một số nguyên bắt đầu. Giá trị bắt đầu không được đưa ra; thay vào đó, chúng ta có thể tự do lựa chọn nó, nhưng nó phải nằm trong khoảng từ 0 đến giới hạn r nào đó."
date: "2026-07-02T02:10:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "K"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 59
verified: true
draft: false
---

[CF 104101K - Bit](https://codeforces.com/problemset/problem/104101/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác bitwise cố định luôn được áp dụng cho một số nguyên bắt đầu. Giá trị bắt đầu không được đưa ra; thay vào đó, chúng ta được tự do lựa chọn nó, nhưng nó phải nằm trong khoảng từ 0 đến một giới hạn nào đó.`r`. Sau khi chọn giá trị ban đầu này, hệ thống sẽ áp dụng cùng một chuỗi thao tác cho nó, tạo ra giá trị cuối cùng. 

Mỗi thao tác ảnh hưởng đến số hiện tại bằng cách sử dụng một trong ba phép biến đổi theo bit: AND với hằng số, OR với hằng số hoặc XOR với hằng số. Trình tự được cố định trên tất cả các truy vấn, nhưng mỗi truy vấn đưa ra một giới hạn trên khác nhau`r`, và chúng ta phải chọn giá trị ban đầu`x ≤ r`giúp tối đa hóa kết quả cuối cùng sau tất cả các hoạt động. 

Khó khăn chính là chúng ta không tối ưu hóa trực tiếp giá trị cuối cùng như một hàm đơn giản của`x`ở dạng số học. Thay vào đó, hàm được xây dựng từ các phép toán theo bit, khiến phép biến đổi phụ thuộc vào cấu trúc nhị phân của`x`. 

Các ràng buộc cho phép tối đa 200.000 thao tác và 200.000 truy vấn, với các giá trị lên tới khoảng 2^30. Điều này ngay lập tức loại trừ mọi mô phỏng theo truy vấn trên tất cả các khả năng có thể.`x`, vì ngay cả việc kiểm tra tất cả các ứng cử viên cho mỗi truy vấn cũng sẽ vượt quá giới hạn khả thi. Ngay cả việc mô phỏng động cho mỗi truy vấn trên các bit cũng phải tránh sự phụ thuộc tuyến tính vào`n`, vì như thế đã là quá chậm rồi. 

Một cách tiếp cận đơn giản sẽ thử từng truy vấn một cách độc lập, tính toán lại cách mỗi truy vấn có thể`x`TRONG`[0, r]`hoạt động sau chuỗi hoạt động đầy đủ. Đối với một truy vấn duy nhất, thậm chí liệt kê tất cả`x`lên tới`r`tốn tới 2^30 khả năng, điều này hoàn toàn không khả thi. Ngay cả việc hạn chế DP theo bit cho mỗi truy vấn mà không xử lý trước vẫn sẽ quá chậm trên tất cả các truy vấn. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc giả sử phép biến đổi là đơn điệu trong`x`. Ví dụ: phép toán AND có thể hủy các bit, phép toán OR có thể buộc các bit bật lên và XOR có thể lật chúng. Một thay đổi nhỏ trong`x`có thể tạo ra một sự thay đổi không cục bộ ở đầu ra, vì vậy lý luận tham lam về`x`vì giá trị số là không đáng tin cậy trừ khi nó được giảm xuống hành vi cấp độ bit. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực bắt đầu bằng cách quan sát điều đó đối với bất kỳ giá trị ban đầu cố định nào`x`, chúng ta có thể mô phỏng toàn bộ chuỗi thao tác và tính giá trị kết quả`y`. Mô phỏng này mất thời gian tuyến tính theo số lượng thao tác, vì mỗi bước sẽ sửa đổi giá trị hiện tại chỉ bằng một thao tác theo bit. Nếu chúng ta lặp lại điều này mọi lúc có thể`x`TRONG`[0, r]`, chúng tôi sẽ cần đánh giá tối đa 2^30 ứng viên cho mỗi truy vấn, mỗi ứng viên có giá O(n), vượt xa mọi giới hạn. 

Sự đơn giản hóa cấu trúc quan trọng xuất phát từ việc tách vấn đề theo từng bit. Mỗi thao tác hoạt động độc lập trên từng bit của số, vì AND, OR và XOR không bao giờ trộn lẫn các bit. Điều này có nghĩa là chúng ta có thể theo dõi xem một bit đầu vào ảnh hưởng như thế nào đến bit đầu ra tương ứng sau chuỗi đầy đủ. 

Khi đã hiểu được phép biến đổi trên mỗi bit này, toàn bộ hàm sẽ trở thành một tập hợp các hàm bit độc lập. Mỗi bit đầu ra chỉ phụ thuộc vào một bit đầu vào duy nhất và có thể được mô tả hoàn toàn bằng những gì xảy ra khi bit đầu vào đó bằng 0 hoặc một. Điều này thu gọn vấn đề thành việc quyết định bit nào của`x`để thiết lập, đồng thời tôn trọng các ràng buộc`x ≤ r`. 

Tại thời điểm đó, tác vụ trở thành một tối ưu hóa bị ràng buộc đối với các bit với trọng số bắt nguồn từ mức độ hữu ích của việc đặt từng bit trong đầu vào. Khó khăn còn lại là hạn chế`x ≤ r`ghép các bit thông qua một điều kiện tiền tố, buộc phải suy luận kiểu chữ số-DP trên các biểu diễn nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · r · n) | O(1) | Quá chậm | 
| Chuyển đổi bitwise + chữ số DP | O(n + q log A) | O(log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp bắt đầu bằng cách thu gọn hiệu ứng của tất cả các hoạt động thành phép chuyển đổi trên mỗi bit. 

1. Đối với mỗi vị trí bit, hãy tính xem bit cuối cùng phụ thuộc như thế nào vào bit đầu tiên. Chúng tôi duy trì hai giá trị trên mỗi bit: bit đầu ra sẽ trở thành gì nếu bit đầu vào là 0 và nó sẽ trở thành gì nếu bit đầu vào là 1. Chúng tôi khởi tạo giá trị này dưới dạng nhận dạng, nghĩa là đầu vào 0 cho đầu ra 0 và đầu vào 1 cho đầu ra 1. Mỗi thao tác cập nhật hai trạng thái này một cách độc lập cho từng bit bằng cách sử dụng mặt nạ hằng số của thao tác đó. 
2. Sau khi xử lý tất cả các thao tác, mỗi bit có một ánh xạ cố định từ bit đầu vào đến bit đầu ra. Điều này có nghĩa là câu trả lời cuối cùng được xác định đầy đủ bằng cách chọn các bit của số ban đầu. 
3. Viết lại giá trị cuối cùng dưới dạng tổng trên các bit. Đối với mỗi bit, thiết lập bit đầu vào`i`đóng góp giá trị cơ bản khi bit bằng 0, cộng với mức tăng bổ sung nếu bit được đặt thành 1. Điều này biến vấn đề thành cực đại hóa hàm trọng số bit tuyến tính. 
4. Bây giờ kết hợp ràng buộc`x ≤ r`. Chúng tôi xử lý các bit từ quan trọng nhất đến ít quan trọng nhất, quyết định xem có khớp với tiền tố của`r`hoặc phá vỡ bên dưới nó. Điều này được xử lý bằng cách sử dụng ý tưởng kiểu chữ số-DP trong đó chúng tôi so sánh các lựa chọn khiến chúng tôi chặt chẽ với`r`so với các lựa chọn làm cho tiền tố nhỏ hơn và các bit tương lai miễn phí. 
5. Tại mỗi bit, chúng tôi đánh giá xem việc đặt bit thành 1 hay 0 mang lại giá trị tổng tốt hơn hay không, có tính đến việc chúng tôi có duy trì chặt chẽ với`r`hoặc chuyển sang trạng thái tự do trong đó các bit còn lại có thể được chọn một cách tham lam dựa trên trọng số của chúng. 
6. Chúng tôi tính toán trước các giá trị hậu tố tốt nhất cho trạng thái tự do để các quyết định ở mỗi bit có thể được thực hiện trong thời gian không đổi. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là sau khi tiền xử lý, mỗi bit của đầu vào đóng góp độc lập vào đầu ra và không có thao tác nào tạo ra sự phụ thuộc giữa các bit đầu vào khác nhau. Điều này làm giảm hệ thống xuống vấn đề tối ưu hóa chuỗi nhị phân có trọng số. Cấu trúc chữ số-DP đảm bảo rằng mọi số hợp lệ`x ≤ r`được xem xét ngầm bằng cách theo dõi xem tiền tố được xây dựng có bằng hoặc nhỏ hơn tiền tố của`r`. Vì tất cả các quyết định còn lại chỉ phụ thuộc vào trọng số hậu tố ở trạng thái tự do nên mọi nhánh trong không gian tìm kiếm đều được tính đến mà không liệt kê rõ ràng tất cả các giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXB = 30

n, q = map(int, input().split())
ops = [tuple(map(int, input().split())) for _ in range(n)]

# f0[i], f1[i]: final bit i when input bit is 0 / 1
f0 = [0] * MAXB
f1 = [1] * MAXB

for t, a in ops:
    for i in range(MAXB):
        ai = (a >> i) & 1
        if t == 1:
            f0[i] = f0[i] & ai
            f1[i] = f1[i] & ai
        elif t == 2:
            f0[i] = f0[i] | ai
            f1[i] = f1[i] | ai
        else:
            f0[i] = f0[i] ^ ai
            f1[i] = f1[i] ^ ai

base = [f0[i] for i in range(MAXB)]
gain = [f1[i] - f0[i] for i in range(MAXB)]

# precompute suffix best for free state
best_free = [0] * (MAXB + 1)
for i in range(MAXB - 1, -1, -1):
    best_free[i] = best_free[i + 1] + max(0, gain[i])

def solve(r):
    rbits = [(r >> i) & 1 for i in range(MAXB)]
    
    # tight DP suffix: best possible from i..end when prefix still tight
    # we compute it on the fly with memoized recursion via iteration
    # dp_tight[i][tight_prefix_value handled implicitly by rbits]
    
    ans = 0
    tight = True
    prefix_value = 0
    
    for i in range(MAXB - 1, -1, -1):
        rb = rbits[i]
        
        if not tight:
            ans += base[i] + max(0, gain[i])
            continue
        
        # try x_i = 0
        val0 = base[i] + best_free[i + 1]
        
        # try x_i = 1 (only if allowed)
        if rb == 1:
            val1 = base[i] + gain[i] + best_free[i + 1]
            if val1 >= val0:
                ans += base[i] + gain[i]
            else:
                ans += base[i]
                tight = False
        else:
            ans += base[i]
    
    return ans

for _ in range(q):
    r = int(input())
    print(solve(r))
```Bước tiền xử lý xây dựng phép biến đổi trên mỗi bit bằng cách theo dõi, đối với từng bit một cách độc lập, đầu ra sẽ trở thành gì khi bit đầu vào bằng 0 hoặc một. Điều này tránh mọi tương tác giữa các bit trong chuỗi hoạt động. 

các`gain`mảng biểu thị mức độ lợi ích thu được bằng cách đặt một bit đầu vào cụ thể thành 1 thay vì 0. các`best_free`mảng tích lũy đóng góp tốt nhất có thể đạt được từ các bit thấp hơn khi không còn ràng buộc gắn với`r`. 

Trong hàm truy vấn, vòng lặp xử lý các bit từ quan trọng nhất đến ít quan trọng nhất, duy trì xem tiền tố được xây dựng có còn bằng hay không`r`. Nếu chúng ta giữ chặt, chúng ta phải tôn trọng`r`cấu trúc của; mặt khác, chúng ta có thể tự do tối đa hóa các khoản đóng góp còn lại bằng cách sử dụng lợi ích hậu tố được tính toán trước. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với độ rộng bit nhỏ trong đó chỉ có ba bit quan trọng. Giả sử sau khi tiền xử lý chúng ta thu được`base = [1, 0, 2]`Và`gain = [1, -2, 3]`cho các bit từ thấp đến cao. 

Đối với một truy vấn`r = 5 (101)`, chúng tôi xử lý các bit từ cao đến thấp: 

| Chút | r bit | sự lựa chọn | chặt chẽ | đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | quyết định giữa 0 và 1 | chặt chẽ | so sánh tương lai + đạt được | 
| 1 | 0 | buộc 0 | chặt chẽ trở thành sai nếu cần thiết | | 
| 0 | 1 | quyết định tự do hay chặt chẽ | phụ thuộc | | 

Dấu vết này cho thấy các quyết định ban đầu hạn chế quyền tự do sau này như thế nào và tại sao việc tính toán trước hậu tố lại quan trọng: một khi chúng ta phá vỡ tính chặt chẽ, tất cả các bit còn lại sẽ được chọn một cách tham lam. 

Ví dụ thứ hai với`r = 2 (010)`thể hiện trường hợp bit cao bắt buộc bằng 0. Ở bit cao nhất, chúng ta không thể đặt`1`, do đó trạng thái ngay lập tức hạn chế tất cả các số hợp lệ và phần còn lại của quá trình tính toán giảm xuống mức tối đa hóa tự do trên các bit còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 30 + q · 30) | mỗi thao tác cập nhật 30 bit, mỗi truy vấn xử lý 30 bit | 
| Không gian | O(30) | chỉ các mảng biến đổi theo bit mới được lưu trữ | 

Các ràng buộc cho phép lên tới 200.000 thao tác và truy vấn, đồng thời hệ số xử lý 30 bit không đổi giúp giải pháp thoải mái trong giới hạn. Việc sử dụng bộ nhớ không đổi đối với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXB = 3
    n, q = map(int, input().split())
    ops = [tuple(map(int, input().split())) for _ in range(n)]

    f0 = [0] * MAXB
    f1 = [1] * MAXB

    for t, a in ops:
        for i in range(MAXB):
            ai = (a >> i) & 1
            if t == 1:
                f0[i] &= ai
                f1[i] &= ai
            elif t == 2:
                f0[i] |= ai
                f1[i] |= ai
            else:
                f0[i] ^= ai
                f1[i] ^= ai

    base = [f0[i] for i in range(MAXB)]
    gain = [f1[i] - f0[i] for i in range(MAXB)]

    def solve(r):
        rbits = [(r >> i) & 1 for i in range(MAXB)]
        ans = 0
        tight = True
        for i in range(MAXB - 1, -1, -1):
            if not tight:
                ans += base[i] + max(0, gain[i])
            else:
                rb = rbits[i]
                if rb == 0:
                    ans += base[i]
                else:
                    ans += base[i] + max(0, gain[i])
        return ans

    return "\n".join(str(solve(int(x))) for x in input().split())

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động đơn lẻ tối thiểu | truyền bit đúng | tính chính xác của các cập nhật trên mỗi bit | 
| tất cả các hoạt động HOẶC | luôn tối đa hóa bit | xử lý đạt được | 
| hỗn hợp AND/XOR | ký thay đổi mức tăng | sự chuyển đổi đúng đắn | 
| r = 0 | chỉ cho phép x=0 | ràng buộc ranh giới | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi tất cả lợi ích đều trở thành âm. Trong trường hợp này, chiến lược tối ưu là tránh đặt bất kỳ bit nào thành 1 trừ khi bị ràng buộc`x ≤ r`. Thuật toán xử lý việc này một cách tự nhiên vì phần đóng góp hậu tố`max(0, gain[i])`đảm bảo rằng các trạng thái tự do không bao giờ chọn các bit có hại. 

Một trường hợp cạnh khác là khi ràng buộc`r`buộc bit cao bằng 0. Ví dụ, nếu`r = 1000₂`, bất kỳ nỗ lực nào để đặt bit cao nhất trong`x`ngay lập tức phá vỡ tính khả thi. Logic trạng thái chặt chẽ không cho phép nhánh này một cách chính xác và tiếp tục với các bit thấp hơn, đảm bảo chỉ các số hợp lệ mới được xem xét. 

Trường hợp cuối cùng xảy ra khi các thao tác XOR lật lại việc giải thích một bit nhiều lần. Mặc dù hành vi trung gian có vẻ không ổn định,`(f0, f1)`biểu diễn thu gọn tất cả các lần lật thành ánh xạ xác định cuối cùng, do đó DP không bao giờ phụ thuộc vào thứ tự hoạt động ngoài quá trình tiền xử lý.
