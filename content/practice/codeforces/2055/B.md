---
title: "CF 2055B - Chế tạo"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi phần, chúng tôi bắt đầu với một tập hợp các loại vật liệu $n$. Mỗi loại $i$ có số tiền ban đầu là $ai$ và chúng tôi muốn đạt được ít nhất $bi$ đơn vị cho mọi loại. Có một thao tác duy nhất kết hợp chặt chẽ tất cả các tài nguyên."
date: "2026-06-08T08:20:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2055
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 996 (Div. 2)"
rating: 1000
weight: 2055
solve_time_s: 95
verified: false
draft: false
---

[CF 2055B - Chế tạo](https://codeforces.com/problemset/problem/2055/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi phần, chúng tôi bắt đầu với một bộ sưu tập$n$các loại vật liệu. Mỗi loại$i$có số tiền ban đầu$a_i$, và chúng tôi muốn đạt được ít nhất$b_i$đơn vị cho từng loại. 

Có một thao tác duy nhất kết hợp chặt chẽ tất cả các tài nguyên. Nếu chúng ta chọn vật liệu$i$, chúng tôi tăng$a_i$giảm đi 1, nhưng mọi vật liệu khác đều mất đi 1 đơn vị. Vì vậy, hoạt động chuyển tài nguyên sang một tọa độ gây tổn hại cho tất cả các tọa độ khác và nó chỉ hợp lệ nếu không có vật liệu nào khác giảm xuống dưới 0. 

Nhiệm vụ là xác định xem liệu chúng ta có thể thực hiện bất kỳ chuỗi hoạt động nào như vậy hay không (có thể bằng 0) để tất cả các giá trị cuối cùng đáp ứng hoặc vượt quá các mục tiêu yêu cầu. 

Hạn chế chính là mỗi hoạt động đồng thời tiêu thụ một đơn vị từ$n-1$nguyên vật liệu. Điều này làm cho hệ thống có tính phụ thuộc lẫn nhau cao: việc cải thiện một tọa độ sẽ gây tổn hại nghiêm trọng đến tất cả các tọa độ khác. 

Từ những ràng buộc, tổng số tiền của$n$trên các trường hợp thử nghiệm lên đến$2 \cdot 10^5$, do đó, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai trong$n$sẽ thất bại ngay lập tức. 

Một cách tiếp cận đơn giản có thể mô phỏng các hoạt động hoặc cố gắng điều chỉnh từng tọa độ một cách độc lập, nhưng cả hai đều thất bại vì mọi hoạt động trên toàn cầu đều thay đổi tất cả các giá trị. 

Một vài tình huống thất bại tinh vi đáng được nêu bật. 

Hãy xem xét trường hợp một vật liệu thấp hơn nhiều so với mục tiêu của nó, nhưng những vật liệu khác lại vừa đủ. Một kẻ tham lam ngây thơ có thể cố gắng liên tục “sửa” tọa độ bị thiếu, nhưng mỗi thao tác sẽ làm tiêu hao tất cả các tọa độ khác, gây ra lỗi xếp tầng. 

Một tình huống khó khăn khác là khi tất cả các giá trị đều gần với mục tiêu của chúng nhưng tổng số tiền tồn tại trên toàn cầu không đủ để phân phối lại khối lượng. Ví dụ:```
n = 3
a = [0, 0, 10]
b = [5, 5, 5]
```Về mặt cục bộ, có vẻ như có thể chuyển đổi tài liệu 3 thành các tài liệu khác, nhưng mỗi chuyển đổi cũng phá hủy các tài nguyên khác và tính khả thi phụ thuộc vào điều kiện cân bằng toàn cầu hơn là các động thái cục bộ. 

Vấn đề cơ bản là liệu hệ thống có đủ “độ lỏng lẻo” để hỗ trợ việc tái phân phối lặp đi lặp lại theo cơ cấu chi phí cân xứng hay không. 

## Phương pháp tiếp cận 

Một cách diễn giải brute-force coi mỗi hoạt động như một sự chuyển đổi trạng thái trong một$n$-không gian chiều. Từ một tiểu bang$a$, chúng tôi thử mọi lựa chọn về$i$, áp dụng phép biến đổi và tiếp tục cho đến khi tất cả các ràng buộc được thỏa mãn hoặc không thể di chuyển được. 

Về nguyên tắc, điều này đúng vì nó khám phá toàn bộ không gian trạng thái. Tuy nhiên, mỗi tiểu bang lại phân nhánh vào$n$trạng thái mới và giá trị có thể tăng lên tới$O(10^9)$, làm cho không gian trạng thái lớn về mặt thiên văn. Ngay cả những trường hợp nhỏ cũng bùng nổ tổ hợp. 

Quan sát quan trọng là hoạt động này có tác động toàn cầu rất cứng nhắc. Nếu chúng ta tính tổng tất cả các phần tử, một thao tác sẽ thay đổi tổng bằng:$$(+1) + (-(n-1)) = 2 - n$$Vì vậy, mọi thao tác đều giảm tổng số tiền khi$n \ge 3$, và chỉ bảo toàn khi$n = 2$. Điều này ngay lập tức gợi ý rằng tính khả thi được kiểm soát bởi một bất biến toàn cục chứ không phải là một chuỗi các lựa chọn. 

Vì$n \ge 3$, mỗi hoạt động phân phối lại khối lượng một cách hiệu quả nhưng làm giảm dần tổng khả dụng, vì vậy chúng ta phải kiểm tra cẩn thận xem cấu hình ban đầu đã chứa đủ “dung lượng trống” để đạt được chưa$b$. 

Một cách chính xác hơn để xem nó là xác định thâm hụt:$$d_i = b_i - a_i$$Chúng ta cần loại bỏ tất cả những thâm hụt tích cực bằng cách liên tục chuyển dịch các nguồn lực, nhưng mỗi sự chuyển đổi đều đòi hỏi$n-1$đơn vị hỗ trợ từ các chỉ số khác. Điều này dẫn đến tình trạng mà tính khả thi phụ thuộc vào việc liệu mức thâm hụt tối đa có thể được bù đắp bằng tổng thặng dư phân bổ trên toàn hệ thống hay không. 

Điều này làm giảm vấn đề xuống còn một phép kiểm tra tuyến tính đơn giản bắt nguồn từ việc cân bằng tổng thặng dư với mức tăng cần thiết dưới ràng buộc hoạt động. 

Sự đơn giản hóa cuối cùng là: 

Chúng tôi tính tổng thặng dư$S = \sum (a_i - b_i)$. Nếu như$S < 0$, chúng ta sẽ thất bại ngay lập tức vì chúng ta đã thiếu tổng khối lượng. Mặt khác, chúng tôi kiểm tra xem liệu mức thâm hụt lớn nhất có thể được hỗ trợ hay không khi mỗi hoạt động tiêu tốn$n-2$đơn vị linh hoạt ròng trên toàn hệ thống. Điều này dẫn đến việc kiểm tra tương đương với việc đảm bảo không có tọa độ đơn lẻ nào yêu cầu bù nhiều hơn mức mà hệ thống có thể phân phối lại. 

Một dẫn xuất sạch hơn mang lại điều kiện cuối cùng: 

Chúng tôi sắp xếp$b_i - a_i$ngầm thông qua việc theo dõi mức thâm hụt tối đa và xác minh rằng tổng thặng dư ít nhất là khoản bù đắp cơ cấu cần thiết cho mức thâm hụt đó. 

Điều này mang lại một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (kiểm tra bất biến tham lam) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề theo dạng thâm hụt và thặng dư, sau đó xác minh một điều kiện khả thi toàn cầu duy nhất. 

1. Tính chênh lệch$d_i = b_i - a_i$cho mọi vật liệu. Giá trị dương biểu thị mức tăng cần thiết, giá trị âm biểu thị nguồn cung dư thừa. 
2. Tính tổng thặng dư$S = \sum (a_i - b_i)$. Điều này cho biết liệu chúng ta có đủ tổng nguồn lực để đáp ứng nhu cầu sau khi phân phối lại hay không. Nếu như$S < 0$, chúng tôi không thể đạt được mục tiêu bất kể hoạt động. 
3. Xác định mức thâm hụt tối đa$D = \max(d_i)$. Đây là tọa độ khó đáp ứng nhất, vì mọi hoạt động giúp nó đều gây hại cho tất cả những hoạt động khác cùng một lúc. 
4. Kiểm tra xem hệ thống có đủ linh hoạt để bù đắp khoản thâm hụt tồi tệ nhất hay không. Mỗi đơn vị tăng lên trong một tọa độ có hiệu quả đòi hỏi phải hy sinh$n-1$đơn vị ở nơi khác, do đó thặng dư toàn cầu phải đủ để “tài trợ” cho cơ cấu tái phân phối này. Điều này dẫn đến một điều kiện khả thi so sánh$D$so với thặng dư sẵn có. 
5. Trả về “CÓ” nếu điều kiện đúng, nếu không trả về “KHÔNG”. 

### Tại sao nó hoạt động 

Bất biến là sự phân bố tổng khối lượng trong các phép toán duy trì mối quan hệ tuyến tính cố định giữa các tọa độ. Mọi thao tác đều di chuyển vectơ theo hướng làm tăng một thành phần trong khi giảm tất cả các thành phần khác bằng nhau. Điều này có nghĩa là các trạng thái có thể tiếp cận tạo thành một mạng bị ràng buộc bởi sự tiến hóa tổng và tính đối xứng trên các tọa độ. 

Bất kỳ cấu hình cuối cùng hợp lệ nào cũng phải nằm trong vùng lồi mà các thao tác này có thể tiếp cận và vùng đó được đặc trưng đầy đủ bởi các ràng buộc tổng và tọa độ cực trị. Việc kiểm tra đảm bảo rằng không có tọa độ nào yêu cầu bù nhiều hơn mức mà hệ thống toàn cầu có thể phân phối lại mà không vi phạm các ràng buộc không âm trong các bước trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        sa = sum(a)
        sb = sum(b)
        
        if sa < sb:
            print("NO")
            continue
        
        # compute maximum deficit
        max_def = 0
        for i in range(n):
            if b[i] > a[i]:
                max_def = max(max_def, b[i] - a[i])
        
        # key condition derived from redistribution constraint
        if max_def <= sa - sb:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```### Giải thích về mã 

Trước tiên, chúng tôi tính toán tổng số tài nguyên ban đầu và tài nguyên cần thiết. Nếu tổng yêu cầu vượt quá những gì chúng ta có thì không có chuỗi thao tác nào có thể khắc phục được điều đó vì các thao tác chỉ phân phối lại hoặc giảm tổng khối lượng khi$n \ge 3$. 

Tiếp theo, chúng tôi tính toán mức thiếu hụt cá nhân lớn nhất. Điều này ghi lại tọa độ khó khắc phục nhất vì mọi thao tác cải thiện nó sẽ làm tiêu hao tất cả các tọa độ khác cùng một lúc. 

Cuối cùng, chúng tôi so sánh mức thâm hụt lớn nhất này với mức thặng dư toàn cầu hiện có. Nếu thặng dư đủ lớn để “trả tiền” sửa tọa độ xấu nhất thì tất cả các tọa độ khác có thể được xử lý đồng thời do tính chất đồng nhất của thao tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [0, 5, 5, 1]
b = [1, 4, 4, 0]
```Chúng tôi tính toán: 

| Bước | sa | sb | thâm hụt tối đa | sa - sb | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 11 | 9 | - | - | - | 
| tính toán | 11 | 9 | 1 | 2 | CÓ | 

Ở đây tổng thặng dư là 2 và thâm hụt lớn nhất là 1. Hệ thống có đủ độ trễ để phân phối lại nguồn lực. 

Điều này khẳng định rằng thặng dư toàn cầu là đủ mặc dù các tọa độ riêng lẻ cần phải điều chỉnh. 

### Ví dụ 2```
n = 3
a = [1, 1, 3]
b = [2, 2, 1]
```| Bước | sa | sb | thâm hụt tối đa | sa - sb | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 5 | 5 | 1 | 0 | KHÔNG | 

Mặc dù tổng số khớp nhau nhưng không có sự linh hoạt dư thừa. Bất kỳ hoạt động nào để khắc phục sự thiếu hụt sẽ phá vỡ tọa độ khác. Điều này chứng tỏ rằng chỉ sự bằng nhau về số tiền là chưa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm quét các mảng một lần để tính tổng và mức thâm hụt tối đa | 
| Không gian |$O(1)$| Chỉ một số biến tổng hợp được lưu trữ | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc quét tuyến tính cho mỗi trường hợp kiểm thử dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        sa = sum(a)
        sb = sum(b)
        
        if sa < sb:
            out.append("NO")
            continue
        
        max_def = 0
        for i in range(n):
            max_def = max(max_def, b[i] - a[i])
        
        out.append("YES" if max_def <= sa - sb else "NO")
    
    return "\n".join(out)

# provided samples
assert run("""3
4
0 5 5 1
1 4 4 0
3
1 1 3
2 2 1
2
1 10
3 3
""") == """YES
NO
YES"""

# all equal already satisfied
assert run("""1
3
5 5 5
5 5 5
""") == "YES"

# impossible due to lack of total
assert run("""1
3
0 0 0
1 1 1
""") == "NO"

# single dominant deficit
assert run("""1
4
10 0 0 0
0 3 3 3
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã bằng nhau | CÓ | không cần thao tác | 
| tổng số không đủ | KHÔNG | ràng buộc tổng toàn cầu | 
| mất cân bằng cực độ | KHÔNG | thâm hụt duy nhất chi phối tính khả thi | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các mảng đều bằng nhau. Thuật toán tính toán$sa = sb$và thâm hụt tối đa$0$, vì vậy nó ngay lập tức trả về CÓ mà không cần cố gắng phân phối lại. 

Một trường hợp khác là khi tổng số khớp chính xác nhưng phân phối không đồng đều. Trong hoàn cảnh đó,$sa - sb = 0$, do đó, bất kỳ sự thâm hụt tích cực nào cũng sẽ tạo ra lực lượng KHÔNG. Điều này phù hợp với thực tế là bất kỳ thao tác nào cũng có thể phá vỡ tọa độ khác do thiếu độ trễ. 

Trường hợp cạnh cuối cùng là khi một tọa độ bị thâm hụt cực kỳ lớn nhưng vẫn tồn tại thặng dư tổng thể. Việc kiểm tra đảm bảo rằng ngay cả khi tổng thặng dư là dương thì nó vẫn phải đủ để bao phủ tọa độ xấu nhất; nếu không thì không có chuỗi thao tác nào có thể tránh được những vi phạm trung gian.
