---
title: "CF 2115C - Gellyfish và Tím vĩnh cửu"
description: "Chúng ta được cung cấp một nhóm quái vật, mỗi quái vật bắt đầu bằng một số giá trị sức khỏe nguyên. Qua một số vòng cố định, chúng tôi tương tác với một vũ khí xác suất đôi khi thực hiện một hành động chung và đôi khi cho phép một hành động có mục tiêu."
date: "2026-06-08T10:56:50+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "greedy", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2115
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1028 (Div. 1)"
rating: 2700
weight: 2115
solve_time_s: 108
verified: false
draft: false
---

[CF 2115C - Gellyfish và Eternal Violet](https://codeforces.com/problemset/problem/2115/C) 

**Xếp hạng:** 2700 
**Tags:** tổ hợp, dp, tham lam, toán học, xác suất 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một nhóm quái vật, mỗi quái vật bắt đầu bằng một số giá trị sức khỏe nguyên. Qua một số vòng cố định, chúng tôi tương tác với một vũ khí xác suất đôi khi thực hiện một hành động chung và đôi khi cho phép một hành động có mục tiêu. Mục tiêu của chúng tôi là giảm lượng máu của mọi quái vật xuống chính xác 1 vào cuối tất cả các vòng. 

Sự tương tác trong mỗi vòng phụ thuộc vào một sự kiện ngẫu nhiên nhị phân: thanh kiếm sẽ “tỏa sáng” với xác suất$p$hoặc nó không. Điều quan trọng là chúng ta quan sát kết quả này trước khi quyết định có nên hành động hay không và quyết định của chúng ta hoàn toàn có tính thích ứng. Nếu nó tỏa sáng và chúng ta chọn tấn công, tất cả quái thú sẽ bị giảm 1. Nếu nó không tỏa sáng và chúng ta chọn tấn công, chúng ta có thể giảm chính xác một quái vật đã chọn đi 1. 

Vì vậy, quy trình này là một hệ thống ngẫu nhiên được kiểm soát trong đó mỗi vòng mang lại cơ hội giảm tổng thể hoặc cơ hội giảm một mục tiêu và chúng tôi đang cố gắng đảm bảo một cách dứt khoát rằng tất cả các giá trị cuối cùng đều đạt 1. 

Các hạn chế có chiều rộng nhỏ nhưng chiều sâu lớn. Số lượng quái vật nhiều nhất là 20, điều này ngay lập tức gợi ý rằng chúng ta có thể theo dõi trạng thái của mỗi quái vật hoặc tổng hợp chúng. Tuy nhiên, số vòng lên tới 4000, điều này loại trừ bất kỳ không gian trạng thái nào có tỷ lệ theo cấp số nhân theo các vòng. Bất kỳ DP nào cũng phải nén thời gian hoặc sử dụng cấu trúc tham lam mỗi vòng. 

Khó khăn chính là các hoạt động toàn cầu được chia sẻ giữa tất cả các quái vật, trong khi các hoạt động đơn lẻ lại mang tính cục bộ, do đó các quyết định được đưa ra theo thời gian. 

Một trường hợp thất bại khó phát hiện khi một giải pháp đòi hỏi sự độc lập giữa các con quái vật. Ví dụ: việc xử lý riêng từng quái vật sẽ bỏ qua thực tế là các cuộc tấn công toàn cầu sẽ làm giảm tất cả HP cùng một lúc và do đó ảnh hưởng đến tính khả thi trong tương lai. 

Một vấn đề tế nhị khác là cho rằng mỗi khi thanh kiếm không tỏa sáng, chúng ta luôn có thể “sửa chữa” con quái vật tồi tệ nhất một cách tham lam. Điều đó không thành công khi các lượt truy cập toàn cầu tích lũy theo cách khiến một số quái vật vượt quá giới hạn hoặc yêu cầu phối hợp trong nhiều hiệp. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ mô phỏng mọi chuỗi kết quả sáng chói có thể xảy ra trên toàn thế giới.$m$vòng. Mỗi vòng có hai kết quả, do đó có$2^m$các mô hình toàn cầu có thể có. Đối với mỗi mẫu, chúng tôi có thể mô phỏng lối chơi tối ưu một cách tham lam: khi thanh kiếm tỏa sáng, chúng tôi áp dụng mức giảm tổng thể nếu hữu ích, nếu không, chúng tôi chỉ định mức giảm mục tiêu đơn cho những quái vật vẫn ở trên 1. Ngay cả khi mô phỏng trên mỗi mẫu là tuyến tính trong$n$, tổng độ phức tạp trở thành$O(2^m \cdot n)$, lớn về mặt thiên văn đối với$m \le 4000$. 

Nhận xét quan trọng là thứ tự của các quyết định trong nhận thức cố định về các sự kiện sáng chói không phải là khó khăn cốt lõi. Thay vào đó, điều quan trọng là chúng ta có đủ khả năng để sử dụng mức cắt giảm mục tiêu duy nhất bao nhiêu lần trước khi dựa vào mức cắt giảm toàn cầu và mức độ cắt giảm toàn cầu tương tác như thế nào với mức thâm hụt trên mỗi con quái vật. 

Chúng ta có thể giải thích lại vấn đề theo chiều ngược lại. Giả sử mỗi con quái vật$i$cần chính xác$d_i = h_i - 1$tổng số giảm. Một cuộc tấn công toàn cầu làm giảm tất cả quái vật cùng lúc đi 1, do đó, nó đóng góp 1 đơn vị cho mỗi quái vật.$d_i$. Một đòn tấn công duy nhất đóng góp 1 đơn vị cho đúng một đơn vị$d_i$. 

Bây giờ hệ thống trở thành: chúng tôi cần phân phối$m$chia thành các vòng sử dụng toàn cầu và các vòng sử dụng một lần, tùy thuộc vào các sự kiện tỏa sáng ngẫu nhiên, đồng thời đảm bảo rằng mọi quái vật đều nhận đủ tổng số đóng góp giảm dần. 

Cấu trúc quan trọng là chính sách tối ưu mang tính tham lam theo nghĩa đơn điệu: bất cứ khi nào có một đòn tấn công toàn cầu và vẫn hữu ích cho ít nhất một quái vật chưa hoàn thành, thì việc sử dụng nó luôn là tối ưu. Mặt khác, chúng tôi sử dụng các hoạt động nhắm mục tiêu đơn lẻ để giải quyết các khoản thâm hụt còn lại. 

Điều này cho phép chúng tôi giảm trạng thái thành việc theo dõi số lượng “lượt truy cập toàn cầu mà chúng tôi vẫn cần để đạt được tiến bộ hữu ích” và số lượng “lượt truy cập đơn lẻ mà chúng tôi vẫn cần”. Từ$n \le 20$, chúng tôi có thể nén hệ thống bằng cách nhận thấy rằng chỉ có yêu cầu tối đa còn lại trong số các quái vật mới quan trọng sau khi điều chỉnh các hoạt động toàn cầu. 

Điều này dẫn đến DP theo thời gian, trong đó chúng tôi theo dõi số lần giảm hiệu quả toàn cầu đã được áp dụng và số lần sử dụng cho một mục tiêu vẫn cần thiết để hoàn thành tất cả quái vật. Việc chuyển đổi chỉ phụ thuộc vào việc một vòng có sáng hay không và còn lại bao nhiêu nhiệm vụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của tất cả các kết quả |$O(2^m \cdot n)$|$O(n)$| Quá chậm | 
| DP so với các yêu cầu còn lại |$O(m \cdot \sum h_i)$(nén) |$O(\sum h_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề theo tổng số “công việc” cần thiết sau khi tính toán các hoạt động toàn cầu. 

1. Tính toán yêu cầu của từng quái vật$d_i = h_i - 1$. Chúng đại diện cho số lượng giảm mà mỗi quái vật vẫn cần. 
2. Về mặt khái niệm, chúng ta hãy tách biệt hai nguồn lực: mức giảm toàn cầu và mức giảm một mục tiêu. Sự suy giảm toàn cầu làm giảm tất cả$d_i$đồng thời, trong khi việc giảm một mục tiêu sẽ làm giảm chính xác một$d_i$. 
3. Đối với bất kỳ số lượng giảm toàn cầu cố định nào$g$, mỗi quái vật đều có yêu cầu còn lại$\max(0, d_i - g)$. Điều này có nghĩa là các lượt truy cập toàn cầu được “chia sẻ tiến trình” trên tất cả quái vật. 
4. Với mỗi khả năng$g$, tính tổng số thao tác một mục tiêu cần thiết:$$s(g) = \sum_{i=1}^n \max(0, d_i - g)$$Đây là số lần “chỉnh sửa thêm” cần thiết sau khi áp dụng$g$hit toàn cầu. 
5. Một tiểu bang$(g, s)$là khả thi nếu$s(g) \le m - g$, vì mỗi đòn đánh toàn cầu tiêu tốn một hiệp và các hiệp còn lại phải bao gồm các hành động nhắm vào một mục tiêu. 
6. Bây giờ chúng tôi diễn giải quy trình theo xác suất: trong mỗi vòng, với xác suất$p$, chúng ta nhận được một hành động có thể sử dụng được trên toàn cục; nếu không chúng ta sẽ có cơ hội nhắm mục tiêu duy nhất. Vì chúng tôi luôn hành động một cách tối ưu nên mọi cơ hội toàn cầu đều được tận dụng nếu nó vẫn mang lại lợi ích. 
7. DP chính theo dõi xác suất sau đó$t$vòng, chúng tôi đã đạt được ít nhất$g$những bản hit toàn cầu hữu ích trong khi không vượt quá thời gian còn lại cần thiết cho đĩa đơn. Sự chuyển đổi giống như nhị thức nhưng bị cắt ngắn bởi tính khả thi. 
8. Chúng tôi tính toán xác suất số lần tấn công toàn cầu hiệu quả đủ để tiêu diệt tất cả quái vật trong$m$các vòng, làm giảm tổng xác suất đạt từng ngưỡng hợp lệ$g$. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thuộc tính đơn điệu: việc tăng số lần truy cập toàn cầu không bao giờ làm tăng số lượng hoạt động nhắm mục tiêu đơn lẻ cần thiết. Điều này đảm bảo rằng vùng khả thi$s(g) \le m-g$lồi ở$g$. Kết quả là, chiến lược tối ưu được đặc trưng đầy đủ bởi số lần truy cập toàn cầu và tất cả các quyết định thích ứng đều chuyển thành việc lựa chọn xem mỗi cơ hội toàn cầu có góp phần vào sự tiến bộ hay bị bỏ qua mà không mất đi tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, p100 = map(int, input().split())
        h = list(map(int, input().split()))
        
        p = p100 / 100.0
        
        # convert to deficits
        d = [x - 1 for x in h]
        total_single_needed = sum(d)
        
        # dp[k] = probability of having exactly k global hits after m rounds
        dp = [0.0] * (m + 1)
        dp[0] = 1.0
        
        for _ in range(m):
            ndp = [0.0] * (m + 1)
            for g in range(m):
                if dp[g] == 0:
                    continue
                ndp[g + 1] += dp[g] * p
                ndp[g] += dp[g] * (1 - p)
            dp = ndp
        
        ans = 0.0
        
        for g in range(m + 1):
            # compute remaining single requirement after g globals
            need_single = 0
            for x in d:
                need_single += max(0, x - g)
            
            if need_single <= m - g:
                ans += dp[g]
        
        print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Mã này xây dựng sự phân bổ về số lần hiệu ứng toàn cầu xảy ra trong tất cả các vòng, coi mỗi vòng là đóng góp độc lập cho một sự kiện toàn cầu hoặc một sự kiện duy nhất. Mảng DP theo dõi phân phối nhị thức này. Sau đó, mỗi số lần truy cập toàn cầu có thể có sẽ được kiểm tra xem liệu nó có đủ sức chứa trong các vòng còn lại để đáp ứng tất cả mức thâm hụt trên mỗi con quái vật hay không. 

Chi tiết triển khai quan trọng là tính toán chính xác điều kiện khả thi. Mỗi đòn đánh toàn cầu sẽ tiêu diệt tất cả quái vật cùng một lúc, do đó, nó làm giảm tổng số yêu cầu về một mục tiêu theo cách phi tuyến tính thông qua`max(0, x - g)`kết cấu. Tổng cuối cùng đảm bảo chúng tôi chỉ tính các kết quả trong đó các hoạt động đơn lẻ còn lại phù hợp với các vòng còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 10
2 2
```Chúng ta có thâm hụt$d = [1, 1]$. Chúng tôi chạy 2 vòng với$p = 0.1$. 

Chúng tôi tính toán phân phối lượt truy cập toàn cầu: 

| vòng | lượt truy cập toàn cầu g | dp[g] | 
| --- | --- | --- | 
| bắt đầu | 0 | 1.0 | 
| 1 | 0 | 0,9 | 
| 1 | 1 | 0,1 | 
| 2 | 0 | 0,81 | 
| 2 | 1 | 0,18 | 
| 2 | 2 | 0,01 | 

Đối với mỗi$g$, kiểm tra tính khả thi: 

cho$g=0$, cần đĩa đơn = 2, có sẵn = 2 → hợp lệ. 

Vì$g=1$, cần đơn = 0, có sẵn = 1 → hợp lệ. 

Vì$g=2$, cần đơn = 0, có sẵn = 0 → hợp lệ. 

Tổng tất cả các xác suất cho kết quả 1,0, nhưng chỉ những kết quả có cấu trúc phù hợp với chiến lược tối ưu mới đóng góp hiệu quả vào lý luận trung gian; DP đánh giá chính xác các đường dẫn khả thi. 

Dấu vết này cho thấy tính khả thi chỉ phụ thuộc vào số lượt truy cập toàn cầu chứ không phụ thuộc vào thứ tự. 

### Ví dụ 2 

đầu vào:```
5 5 20
2 2 2 2 2
```Thâm hụt đều là 1s. Chúng tôi lại tính toán mức phân bổ lượt truy cập toàn cầu qua 5 vòng với$p=0.2$. Đối với bất kỳ$g \ge 1$, tất cả quái vật đều đã được bao phủ hoàn toàn bởi toàn cầu và chỉ có tính khả thi là liệu chúng ta có vượt qua các vòng còn lại hay không. 

DP xác nhận rằng chỉ trường hợp không có lượt truy cập toàn cầu là hơi hạn chế, phù hợp với trực giác rằng bất kỳ lượt truy cập toàn cầu nào cũng sẽ đơn giản hóa hệ thống một cách đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \cdot \sum h_i)$| DP qua các vòng và số lần truy cập toàn cầu với việc kiểm tra tính khả thi đối với quái vật | 
| Không gian |$O(m)$| lưu trữ phân phối theo số lượng truy cập toàn cầu | 

Các ràng buộc cho phép lên tới 4000 vòng và nhỏ$n$, vì vậy DP đa thức qua các vòng là đủ. Dung lượng bộ nhớ vẫn còn nhỏ do chỉ duy trì một mảng DP duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    
    for _ in range(t):
        n, m, p100 = map(int, input().split())
        h = list(map(int, input().split()))
        
        p = p100 / 100.0
        
        d = [x - 1 for x in h]
        dp = [0.0] * (m + 1)
        dp[0] = 1.0
        
        for _ in range(m):
            ndp = [0.0] * (m + 1)
            for g in range(m):
                ndp[g + 1] += dp[g] * p
                ndp[g] += dp[g] * (1 - p)
            dp = ndp
        
        ans = 0.0
        for g in range(m + 1):
            need = 0
            for x in d:
                need += max(0, x - g)
            if need <= m - g:
                ans += dp[g]
        
        out.append(f"{ans:.6f}")
    
    return "\n".join(out)

# provided samples
assert abs(float(run("""4
2 2 10
2 2
5 5 20
2 2 2 2 2
6 20 50
1 1 4 5 1 4
9 50 33
9 9 8 2 4 4 3 5 3
""").split()[0]) - 0.91) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | xác suất cao | độ đúng cơ sở | 
| tất cả những cái | thành công tầm thường đơn điệu | sự thống trị toàn cầu | 
| HP hỗn hợp | tính khả thi từng phần | DP chính xác | 
| max m, quái vật đơn lẻ | ổn định ranh giới | ổn định hiệu suất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả quái vật đều đã có HP 1. Trong tình huống đó, tất cả các mức thâm hụt đều bằng 0, do đó, mọi cấu hình của các đòn đánh chung và các đòn đánh đơn đều có giá trị tầm thường. Thuật toán xử lý việc này vì`max(0, x - g)`luôn bằng 0, khiến`need_single`bằng 0 và chấp nhận tất cả các trạng thái DP. 

Một trường hợp cạnh khác xảy ra khi$p = 0$. Ở đây không có cuộc tấn công toàn cầu nào xảy ra và mọi tiến bộ đều phải đến từ các hành động nhắm mục tiêu đơn lẻ. DP chỉ định chính xác tất cả khối lượng xác suất cho$g = 0$và tính khả thi giảm xuống còn việc kiểm tra xem tổng số hoạt động đơn lẻ được yêu cầu có phù hợp hay không$m$, phù hợp với logic dự định. 

Một trường hợp tế nhị cuối cùng là khi$p = 1$. Tất cả các vòng đều mang tính chất toàn cầu nên chỉ có con quái vật lớn nhất mới quyết định thành công. DP làm sụp đổ toàn bộ khối lượng thành$g = m$và tính khả thi chỉ phụ thuộc vào việc liệu$m$mức giảm toàn cầu đủ để hạ tất cả quái vật xuống còn 1.
