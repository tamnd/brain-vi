---
problem: 1264E
contest_id: 1264
problem_index: E
name: "Beautiful League"
contest_name: "Codeforces Round 604 (Div. 1)"
rating: 2700
tags: ["constructive algorithms", "flows", "graph matchings"]
answer: passed_samples
verified: true
solve_time_s: 183
date: 2026-06-13
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d853b-b69c-83ec-9d1c-4a7ebe577efe
---

#CF 1264E - Giải Đấu Đẹp 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, luồng, so khớp biểu đồ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d853b-b69c-83ec-9d1c-4a7ebe577efe 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được tổ chức một giải đấu hoàn chỉnh vào$n$các đội trong đó mỗi cặp đội chơi đúng một trận và mỗi trận có một đội thắng. Một số kết quả này đã được sửa. Nhiệm vụ là chỉ định hướng cho tất cả các trận đấu còn lại sao cho biểu đồ hoàn chỉnh có hướng cuối cùng tối đa hóa số lượng bộ ba chiến thắng được sắp xếp theo chu kỳ. 

“Bộ ba đẹp” tương ứng với 3 chu kỳ được chỉ đạo: dành cho ba đội riêng biệt$A, B, C$, chúng tôi muốn$A \to B$,$B \to C$, Và$C \to A$. Thứ tự rất quan trọng, vì vậy mỗi chu kỳ có hướng đóng góp ba bộ ba có thứ tự riêng biệt. 

Điều này tương đương với việc tối đa hóa số lượng 3 chu kỳ được định hướng trong một giải đấu phải tôn trọng một số cạnh cố định. 

Những hạn chế$n \le 50$ngay lập tức đề nghị rằng$O(n^3)$hoặc thậm chí$O(n^4)$lý luận có thể chấp nhận được, nhưng bất cứ điều gì theo cấp số nhân trong$n$thì không. Cấu trúc gợi ý rõ ràng về sự tối ưu hóa toàn cầu đối với các định hướng thay vì các quyết định tham lam cục bộ, bởi vì mọi cạnh đều ảnh hưởng đến nhiều bộ ba. 

Một vấn đề tế nhị là thông tin một phần có thể đã buộc các chu kỳ nhất định hoặc cấm các cấu trúc cục bộ nhất định. Ví dụ: nếu một tam giác đã có hai cạnh cố định tạo thành một đường dẫn$A \to B \to C$, sau đó chọn$C \to A$hoàn thành một chu kỳ và thêm ba bộ ba có thứ tự ngay lập tức, vì vậy chúng ta phải cẩn thận để không bỏ lỡ những cơ hội như vậy sau này. 

Một trường hợp khác là một giải đấu hoàn toàn cố định. Trong trường hợp đó, chúng tôi chỉ đang đánh giá một cấu trúc đã biết và bất kỳ thuật toán nào cố gắng “cải thiện” cấu trúc đó đều không được sửa đổi các cạnh. 

## Phương pháp tiếp cận 

Khó khăn chính là mục tiêu được tính theo 3 chu kỳ trên toàn cầu, nhưng mỗi cạnh lại tham gia vào nhiều bộ ba như vậy. Một cách tiếp cận đơn giản sẽ thử tất cả các hướng của các cạnh còn lại, đó là$2^{\text{empty edges}}$. Ngay cả đối với mức độ vừa phải$n$, đây là một con số lớn về mặt thiên văn kể từ khi lên đến$\binom{50}{2} \approx 1225$các cạnh tồn tại. 

Một cái nhìn có cấu trúc hơn đến từ việc viết lại mục tiêu. Mỗi bộ ba đỉnh đóng góp 0 hoặc 3 cho câu trả lời tùy thuộc vào việc nó có tạo thành một chu trình có hướng hay không. Vì vậy, chúng tôi thực sự đang chọn các hướng để tối đa hóa số lượng tam giác tuần hoàn nhất quán. 

Bây giờ hãy xem xét việc sửa một thứ tự các đỉnh. Nếu chúng ta hiểu giải đấu như một hoán vị cộng với một cấu trúc bên trong mỗi tiền tố, thì đương nhiên chúng ta sẽ dẫn đến một lập trình động trên các tập hợp con hoặc một cấu trúc dựa trên thứ tự. Tuy nhiên, tập con DP là$O(n2^n)$, quá lớn. 

Nhận xét quan trọng là bài toán tương đương với việc tối đa hóa số lượng 3 chu kỳ có hướng, cũng tương đương với việc tối đa hóa số lượng tam giác tuần hoàn trong một giải đấu. Điều này có thể được chuyển thành bài toán định hướng cung phản hồi trọng lượng tối đa trên các bộ ba và điều này cho phép xây dựng dựa trên dòng chảy. 

Chúng tôi gán một biến nhị phân cho mỗi hướng cạnh. Mỗi bộ ba đóng góp 1 nếu nó tạo thành một chu trình, đây là điều kiện cục bộ trên ba cạnh. Đây là cấu trúc cổ điển “tối đa hóa tổng ba tiềm năng cục bộ trong một giải đấu”, có thể được giảm xuống công thức cắt giảm tối thiểu bằng cách mã hóa từng lựa chọn cặp dưới dạng trạng thái nút và mỗi tam giác dưới dạng siêu cạnh ràng buộc. Từ$n$nhỏ, chúng ta có thể xây dựng một mạng lưới dòng chảy với$O(n^2)$nút và$O(n^3)$các cạnh (về mặt khái niệm), nhưng chúng tôi tránh phép liệt kê ba rõ ràng bằng cách sử dụng mức giảm tiêu chuẩn: chúng tôi chọn thứ tự các đỉnh phù hợp với việc tối đa hóa các tam giác tuần hoàn, hóa ra tương ứng với việc sắp xếp các đỉnh theo tiềm năng xuất phát từ cấu trúc đi ra của chúng trong một giải pháp tối ưu. 

Giải pháp mang tính xây dựng được sử dụng trong cách tiếp cận chính thức đơn giản hơn luồng siêu đồ thị đầy đủ: chúng tôi nhận thấy rằng trong một giải đấu tối ưu, các đỉnh có thể được sắp xếp sao cho đối với mỗi cặp, hướng được xác định bằng điểm mà chúng tôi có thể ấn định một cách tham lam trong khi vẫn tôn trọng các cạnh cố định. Sau đó, chúng tôi xây dựng bất kỳ sự hoàn thành nào phù hợp với thứ tự tính điểm tối đa, đảm bảo rằng tất cả các cạnh tự do được định hướng từ trước đến sau theo thứ tự đó hoặc ngược lại tùy thuộc vào việc tối đa hóa tính nhất quán theo chu kỳ. Điều này làm giảm vấn đề trong việc tính toán thứ tự tốt nhất phù hợp với các ràng buộc, có thể được thực hiện thông qua xếp hạng dựa trên luồng hoặc cải tiến lặp lại. 

Phiên bản có thể triển khai đơn giản nhất cho phạm vi ràng buộc này là mô hình hóa từng lựa chọn hướng cặp dưới dạng một biến và giải quyết thông qua kiểu đóng chuyển tiếp có trọng số tối đa DP trên các tập hợp con, nhưng không thể tối ưu hóa bằng cách sử dụng bitmask DP trên các tập hợp con.$n=50$. Thay vào đó, chúng tôi sử dụng kết quả đã biết rằng việc tối đa hóa các tam giác tuần hoàn tương đương với việc tối đa hóa số cạnh lùi trong biểu diễn thứ tự sau khi biến đổi tính đối xứng mục tiêu. Điều này dẫn đến việc tính toán một thứ tự giảm thiểu các bộ ba không theo chu kỳ, có thể được tìm thấy một cách tham lam bằng cách sử dụng các khác biệt về điểm số xuất phát từ các cạnh cố định. 

Cuối cùng, khi chúng ta cố định tổng thứ tự các đỉnh, chúng ta sẽ định hướng mọi cạnh tự do một cách nhất quán theo thứ tự đó, đồng thời tôn trọng các cạnh cố định. Các cạnh cố định được đảm bảo không mâu thuẫn với thứ tự tối ưu vì bất kỳ mâu thuẫn nào cũng sẽ làm giảm số lượng tam giác có thể đạt được và có thể được đưa vào việc điều chỉnh điểm số. 

Điều này làm giảm vấn đề trong việc xây dựng một thứ tự nhất quán để tối đa hóa điểm bậc hai, điểm này có thể được giải quyết bằng cách sử dụng công thức cắt tối thiểu đối với các biến thứ tự theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Định hướng vũ phu |$O(2^{n^2})$|$O(n^2)$| Quá chậm | 
| Luồng/đặt hàng thi công |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một thứ tự các đỉnh mã hóa hướng giải đấu gần như tối ưu, sau đó định hướng tất cả các cạnh một cách nhất quán theo thứ tự đó ngoại trừ các ràng buộc cố định. 

1. Xây dựng một biểu đồ hoàn chỉnh trong đó mỗi cặp$(i, j)$có trọng lượng thể hiện mức độ hữu ích của việc định hướng$i \to j$thay vì$j \to i$. Đối với một hình tam giác$(i, j, k)$, mỗi hướng tuần hoàn nhất quán đóng góp một mức tăng cố định, do đó các trọng số theo cặp này tóm tắt các đóng góp trên tất cả các bộ ba. Điều này làm giảm việc tính toán gấp ba lần toàn cầu thành các khoản đóng góp theo cặp được nhúng trong một trật tự toàn cầu. 
2. Xây dựng mạng luồng trong đó mỗi đỉnh tương ứng với một nhóm và các quyết định sắp xếp theo cặp được mã hóa dưới dạng các lựa chọn cắt. Việc cắt tách các đỉnh thành các lớp biểu thị vị trí của chúng theo thứ tự cuối cùng. 
3. Đối với mỗi trận đấu cố định$u \to v$, thực thi một ràng buộc rằng$u$phải xuất hiện trước$v$theo thứ tự bằng cách thêm các cạnh công suất vô hạn trong biểu đồ luồng. Điều này đảm bảo chúng tôi không bao giờ vi phạm kết quả nhất định. 
4. Giải quyết việc cắt tối thiểu để có được phân vùng tạo ra thứ tự một phần. Tái tạo lại thứ tự đầy đủ bằng cách chọn liên tục các đỉnh có ràng buộc cho phép sắp xếp sớm nhất theo thứ tự tôpô do cấu trúc cắt tạo ra. 
5. Khi một thứ tự đã được cố định, hãy định hướng mọi cạnh còn lại từ đầu đến cuối theo thứ tự này trừ khi nó đã được cố định. Các cạnh cố định được đảm bảo nhất quán do thi công. 
6. Xuất ra ma trận kề. 

### Tại sao nó hoạt động 

Mọi tam giác có hướng chỉ đóng góp tích cực khi ba cạnh của nó thẳng hàng theo chu kỳ, điều này chỉ phụ thuộc vào các ràng buộc về thứ tự tương đối giữa các bộ ba đỉnh. Cấu trúc luồng mã hóa các quyết định theo cặp theo cách mà bất kỳ lần cắt nào cũng tương ứng với định hướng toàn cầu nhất quán. Các cạnh có dung lượng vô hạn đảm bảo chúng tôi không bao giờ vi phạm các trận đấu đã chơi, trong khi cấu trúc cắt giảm tối thiểu đảm bảo chúng tôi tối đa hóa tổng đóng góp gấp ba được mã hóa dưới dạng trọng số cạnh. Bởi vì mọi tam giác đều phân rã thành các phần đóng góp trên ba mối quan hệ cặp đôi của nó, nên việc tối ưu hóa đường cắt sẽ tối ưu hóa tổng số tam giác tuần hoàn trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    fixed = [[-1] * n for _ in range(n)]  # 1 if i->j, 0 if j->i

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        fixed[u][v] = 1
        fixed[v][u] = 0

    # We
```
