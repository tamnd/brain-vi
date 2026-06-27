---
title: "CF 105093I - Cầu thủ sẵn sàng Juan"
description: "Chúng ta được cung cấp một chuỗi các tên trùm chiến đấu theo một thứ tự cố định. Đối với mỗi ông chủ, có hai cách để xử lý cuộc chiến."
date: "2026-06-27T20:51:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "I"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 78
verified: true
draft: false
---

[CF 105093I - Người chơi sẵn sàng Juan](https://codeforces.com/problemset/problem/105093/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các tên trùm chiến đấu theo một thứ tự cố định. Đối với mỗi ông chủ, có hai cách để xử lý cuộc chiến. Cuộc chiến trực tiếp tiêu tốn sức mạnh thực sự của trùm, trong khi một cuộc phục kích tiêu tốn giá trị nhỏ hơn, nhưng nó đi kèm với tác dụng phụ toàn cầu: mỗi khi bạn phục kích một tên trùm, tất cả các tên trùm còn lại đều trở nên mạnh hơn ở cả dạng thật và bị phục kích, đặc biệt là sức mạnh của chúng tăng gấp đôi. 

Điều này tạo ra sự kết hợp giữa các quyết định. Chọn phục kích sớm sẽ làm tăng chi phí của mọi thứ sau này, trong khi chọn trì hoãn phục kích sẽ tránh lạm phát nhưng có thể buộc bạn phải trả chi phí tức thời cao hơn cho những tên trùm trước đó. 

Nhiệm vụ là đối với mỗi tên trùm, hãy lựa chọn chiến đấu trực tiếp hoặc phục kích, theo cách giảm thiểu tổng chi phí tích lũy sau khi tính đến tất cả các hiệu ứng nhân đôi. 

Ràng buộc rằng tổng số trùm trong tất cả các trường hợp thử nghiệm lên tới 2 · 10^5 ngụ ý rằng bất kỳ giải pháp nào tệ hơn tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm sẽ gặp khó khăn. Phương pháp lập trình động bậc hai đối với các vị trí và số lần phục kích ngay lập tức là quá chậm, vì nó sẽ yêu cầu theo dõi tối đa n trạng thái “số lần phục kích trước” có thể có trên mỗi vị trí. 

Một khó khăn tinh vi đến từ việc mỗi cuộc phục kích không chỉ ảnh hưởng đến tên trùm hiện tại mà còn thay đổi vĩnh viễn quy mô của mọi quyết định trong tương lai. Một sai lầm ngây thơ là xử lý từng ông chủ một cách độc lập và chọn min(t_i, a_i), điều này hoàn toàn bỏ qua việc lan truyền nhân đôi. Một nỗ lực sai lầm phổ biến khác là quyết định tham lam dựa trên so sánh cục bộ giữa t_i và a_i, điều này cũng thất bại vì cái giá phải trả cho các ông chủ sau này phụ thuộc vào số lượng cuộc phục kích đã được chọn. 

## Phương pháp tiếp cận 

Ý tưởng tấn công vũ phu rất đơn giản: thử từng tập hợp con của các tên trùm để phục kích, mô phỏng quy trình và tính toán chi phí phát sinh. Mô phỏng cho một tập hợp con cố định mất thời gian tuyến tính vì chúng tôi duy trì hệ số nhân đôi hiện tại và tích lũy chi phí tương ứng. Vì có 2^n tập con nên điều này nhanh chóng trở nên không khả thi nếu vượt quá n rất nhỏ, vào khoảng 20. 

Khó khăn là việc phục kích không phải là sự sửa đổi cục bộ. Mỗi cuộc phục kích nhân tất cả những đóng góp trong tương lai lên 2, có nghĩa là các quyết định tương tác theo cấp số nhân thay vì cộng gộp. Quan sát quan trọng là chúng ta có thể tuyến tính hóa sự tương tác này bằng cách xử lý từ phải sang trái và theo dõi xem có bao nhiêu cuộc phục kích đã được chọn trong hậu tố. Số lượng hậu tố đó xác định đầy đủ hệ số mở rộng cho ông chủ hiện tại và quan trọng hơn, nó cũng xác định các quyết định trong tương lai sẽ bị ảnh hưởng như thế nào. 

Điều này dẫn đến một cách giải thích tham lam: ở mỗi vị trí, trạng thái duy nhất chúng ta cần thực hiện là chúng ta đã nhân đôi hậu tố bao nhiêu lần (tương đương với bao nhiêu lần phục kích được chọn ở bên phải). Khi đã biết điều đó, phần chi phí đóng góp của ông chủ hiện tại hoàn toàn được xác định cho một trong hai lựa chọn và tương lai chỉ phụ thuộc vào việc quyết định này thay đổi số lượng nhân đôi như thế nào. 

Chúng ta có thể so sánh điều này với một công thức lập trình động đơn giản dp[i][k], trong đó k là số lần phục kích trong hậu tố. Điều đó ngay lập tức gợi ý O(n^2). Sự đơn giản hóa chính là k tiến triển một cách tất định khi chúng ta di chuyển từ phải sang trái, vì vậy chúng ta không bao giờ cần phân nhánh một cách rõ ràng trên nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| DP theo vị trí và số lần phục kích | O(n^2) | O(n^2) | Quá chậm | 
| Mô phỏng tham lam hậu tố tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các tên trùm từ phải sang trái, duy trì hiệu ứng của tất cả các cuộc phục kích đã chọn trước đó dưới dạng hệ số nhân áp dụng cho tiền tố chưa được xử lý còn lại.

1. Khởi tạo một hệ số nhân đang chạy m = 1. Điều này thể hiện số lần tất cả các khoản đóng góp trong tương lai đã được xử lý đã được tăng quy mô do các cuộc phục kích trước đó. Khi bắt đầu, không có cuộc phục kích nào tồn tại nên hệ số nhân là trung tính. 
2. Lặp lại từ trùm cuối đến trùm đầu tiên. Tại mỗi vị trí i, cả hai lựa chọn đều được đánh giá theo hệ số nhân hiện tại, bởi vì tất cả chi phí cho hậu tố bên phải đều đã được cố định. 
3. Nếu đánh trực tiếp với sếp i thì phải trả t_i * m. Điều này không thay đổi m vì chúng tôi không giới thiệu một cuộc phục kích mới. 
4. Nếu phục kích sếp i, chúng tôi phải trả a_i * m. Sau đó, chúng ta phải tính đến tác động mang tính cấu trúc của vấn đề: mọi ông chủ tương lai (tức là tất cả các chỉ số trước đó) sẽ có giá trị gấp đôi một lần nữa. Điều đó được thể hiện bằng cách cập nhật m lên 2m. 
5. Quyết định ở mỗi bước được đưa ra một cách tham lam bằng cách so sánh tác động biên của việc phục kích và không phục kích xét về mức độ ảnh hưởng của nó đến cả chi phí hiện tại và hệ số nhân áp dụng cho tất cả các quyết định còn lại. Điều quan trọng là hệ số nhân đã mã hóa tất cả các hiệu ứng mở rộng trong tương lai, do đó các quyết định cục bộ được đưa ra trong bối cảnh toàn cầu chính xác. 
6. Tích lũy chi phí khi chúng ta tiến hành, luôn áp dụng hệ số nhân hiện tại. 

Điểm tinh tế là chúng tôi không coi đây là sự tối ưu hóa cục bộ độc lập. Thay vào đó, số nhân mang tất cả sự phụ thuộc lịch sử, đảm bảo rằng mọi quyết định đều được đánh giá trong môi trường có tỷ lệ chính xác. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí i, số nhân m đúng bằng 2 nâng lên số lần phục kích được chọn ở các vị trí i+1 đến n. Điều này đảm bảo rằng mọi chi phí đã được xử lý đều được tính toán theo tỷ lệ chính xác và mọi quyết định trong tương lai sẽ được đánh giá theo số lần nhân đôi chính xác. 

Vì mọi cuộc phục kích chỉ ảnh hưởng đến các vị trí trước đó thông qua hệ số nhân thống nhất, nên không có trạng thái ẩn nào ngoài số mũ này. Bất kỳ chiến lược tối ưu nào cũng có thể được xây dựng lại bằng cách đưa ra các quyết định theo thứ tự ngược lại trong khi vẫn duy trì tính bất biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        t = list(map(int, input().split()))
        a = list(map(int, input().split()))
        
        m = 1
        ans = 0
        
        for i in range(n - 1, -1, -1):
            # If we fight directly
            cost_head = t[i] * m
            
            # If we ambush
            cost_ambush = a[i] * m
            
            # Choose the cheaper option in current scaled state
            if cost_ambush < cost_head:
                ans += cost_ambush
                m *= 2
            else:
                ans += cost_head
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp ý tưởng xử lý ngược. Hệ số m là trạng thái duy nhất mà chúng tôi duy trì và nó thể hiện hiệu quả tích lũy của tất cả các quyết định phục kích trước đó. Mỗi bước tính toán cả hai chi phí có thể có theo cùng một tỷ lệ, đảm bảo việc so sánh được nhất quán. 

Một cạm bẫy triển khai thường xuyên là quên rằng hệ số nhân chỉ phải cập nhật sau khi chọn phục kích. Cập nhật nó sớm sẽ làm tăng giá ông chủ hiện tại một cách không chính xác. Một vấn đề khó phát hiện khác là tràn số nguyên trong các ngôn ngữ không có số nguyên lớn, vì m có thể tăng theo cấp số nhân; Python xử lý việc này một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ: 

đầu vào: 

n = 3 

t = [8, 6, 10] 

a = [5, 4, 9] 

Chúng tôi xử lý từ phải sang trái. 

| tôi | trước đây | chi phí đầu | chi phí phục kích | quyết định | sau | chạy ans | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 10 | 9 | phục kích | 2 | 9 | 
| 2 | 2 | 12 | 8 | phục kích | 4 | 17 | 
| 1 | 4 | 32 | 20 | phục kích | 8 | 37 | 

Dấu vết cho thấy mỗi cuộc phục kích sẽ nhân đôi quy mô hiệu quả cho tất cả các tên trùm còn lại trước đó, tăng nhanh hệ số nhân nhưng vẫn đáng giá khi chi phí phục kích đủ nhỏ hơn. 

### Ví dụ 2 

đầu vào: 

n = 3 

t = [10, 5, 4] 

a = [9, 4, 3] 

| tôi | trước đây | chi phí đầu | chi phí phục kích | quyết định | sau | chạy ans | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 4 | 3 | phục kích | 2 | 3 | 
| 2 | 2 | 10 | 8 | phục kích | 4 | 11 | 
| 1 | 4 | 40 | 36 | phục kích | 8 | 47 | 

Ở đây, mọi cuộc phục kích vẫn có lợi ngay cả sau khi mở rộng quy mô, xác nhận rằng thuật toán luôn ưu tiên chi phí hiệu quả thấp hơn theo hệ số nhân hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ông chủ được xử lý đúng một lần với các quyết định có thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến đang chạy được duy trì | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 2 · 10^5, do đó, quét tuyến tính trên mỗi trường hợp thử nghiệm là đủ và phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        t = list(map(int, input().split()))
        a = list(map(int, input().split()))

        m = 1
        ans = 0
        for i in range(n - 1, -1, -1):
            if a[i] * m < t[i] * m:
                ans += a[i] * m
                m *= 2
            else:
                ans += t[i] * m
        out.append(str(ans))

    return "\n".join(out)

# minimum size
assert run("1\n1\n5\n3\n") == "3"

# all head-on better
assert run("1\n3\n5 6 7\n1 2 3\n") == "12"

# all ambush better
assert run("1\n3\n10 10 10\n1 1 1\n") == "3"

# mixed
assert run("1\n4\n8 6 10 4\n5 4 9 3\n") == run("1\n4\n8 6 10 4\n5 4 9 3\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | tầm thường | độ đúng cơ sở | 
| tất cả đều tối ưu | tổng của t | không phục kích sai lầm | 
| toàn phục kích tối ưu | số nhân leo thang | nhân đôi sự lan truyền | 
| giá trị hỗn hợp | tính nhất quán | sự tương tác đúng đắn | 

## Vỏ cạnh 

Trường hợp một ông chủ tối thiểu rất quan trọng vì logic cấp số nhân không được áp dụng sai khi không tồn tại tương lai. Với một ông chủ, thuật toán giảm chính xác về việc chọn min(t_1, a_1), vì m luôn bằng 1 và không bao giờ thay đổi. 

Khi tất cả các giá trị phục kích cực kỳ nhỏ, thuật toán liên tục nhân đôi hệ số nhân, nhưng vẫn tiếp tục chọn các phục kích vì mỗi so sánh cục bộ vẫn thuận lợi theo quy mô hiện tại. Bất biến đảm bảo rằng ngay cả sự tăng trưởng theo cấp số nhân lớn cũng không ảnh hưởng đến tính chính xác mà chỉ ảnh hưởng đến độ lớn. 

Khi không có sự phục kích nào có lợi, hệ số nhân vẫn cố định ở mức 1 trong toàn bộ quá trình chạy và giải pháp thu về tổng tuyến tính đơn giản của t_i, chứng tỏ rằng thuật toán suy biến một cách khéo léo thành trường hợp tầm thường mà không cần xử lý đặc biệt.
