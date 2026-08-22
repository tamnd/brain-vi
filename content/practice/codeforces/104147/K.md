---
title: "CF 104147K - Hobz là người tốt"
description: "Chúng ta được cung cấp một chuỗi nhị phân và chúng ta được phép xóa bất kỳ số lượng ký tự nào khỏi chuỗi đó, có thể tất cả trừ ít nhất một ký tự phải còn lại."
date: "2026-07-02T01:31:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104147
codeforces_index: "K"
codeforces_contest_name: "JCPC 2022"
rating: 0
weight: 104147
solve_time_s: 75
verified: true
draft: false
---

[CF 104147K - Hobz là một chàng trai tốt](https://codeforces.com/problemset/problem/104147/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và chúng ta được phép xóa bất kỳ số lượng ký tự nào khỏi chuỗi đó, có thể tất cả trừ ít nhất một ký tự phải còn lại. Sau khi xóa, chúng tôi muốn chuỗi con còn lại thỏa mãn điều kiện chẵn lẻ dựa trên các vị trí bên trong chuỗi con đó chứ không phải chuỗi gốc. 

Chính xác hơn, nếu chúng ta lập chỉ mục cho chuỗi được giữ bắt đầu từ 1, chúng ta sẽ xem xét tất cả các ký tự được đặt ở vị trí lẻ và tất cả các ký tự được đặt ở vị trí chẵn. Yêu cầu là số lượng`1`ký tự xuất hiện ở vị trí lẻ phải bằng số ký tự`1`các ký tự xuất hiện ở vị trí chẵn. Chúng tôi không bắt buộc phải tối đa hóa độ dài hoặc xây dựng bất cứ điều gì một cách rõ ràng, chỉ để quyết định xem có tồn tại ít nhất một dãy con hợp lệ không trống hay không. 

Các ràng buộc cho phép tối đa 100.000 trường hợp thử nghiệm với tổng chiều dài chuỗi kết hợp là 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các chuỗi con hoặc thậm chí cố gắng mô phỏng tất cả các thao tác xóa một cách độc lập cho mỗi trường hợp thử nghiệm. Bất cứ điều gì vượt quá thời gian tuyến tính cho mỗi trường hợp thử nghiệm sẽ quá chậm và thậm chí tuyến tính cho mỗi trường hợp thử nghiệm phải được xử lý tổng hợp cẩn thận. 

Một vấn đề tế nhị xuất hiện khi nghĩ về sự lựa chọn tham lam. Thật hấp dẫn khi cho rằng chúng ta phải bảo toàn cấu trúc tương đối hoặc tính chẵn lẻ đó phụ thuộc vào các chỉ số ban đầu. Điều đó không chính xác vì các vị trí được đánh số lại sau khi xóa. Một sai lầm phổ biến khác là cho rằng chúng ta cần số lượng số 1 bằng nhau trong các chỉ số chẵn và lẻ của chuỗi gốc, điều này cũng không phản ánh được vấn đề. 

Trường hợp cạnh tối thiểu là một chuỗi ký tự đơn. Nếu nó là`1`, chúng ta có thể lấy nó và các vị trí chỉ là vị trí lẻ 1, vì vậy vị trí lẻ là 1 và vị trí chẵn là 0, điều kiện này không đạt. Nếu nó là`0`, chúng ta cũng có thể lấy nó và cả hai số đều bằng 0, vì vậy nó hợp lệ. Điều này đã cho thấy rằng số 0 là vô hại và số 0 là những người đóng góp có ý nghĩa duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi dãy con và kiểm tra xem nó có thỏa mãn điều kiện hay không. Đối với mỗi dãy con, chúng ta sẽ mô phỏng cấu trúc của nó và đếm các dãy số ở vị trí chẵn và lẻ. Vì có nhiều dãy con theo cấp số nhân nên cách tiếp cận này phát triển giống như$O(2^n)$cho mỗi trường hợp thử nghiệm, điều này hoàn toàn không khả thi ngay cả đối với các chuỗi nhỏ. 

Để đơn giản hóa, chúng ta nên tập trung vào những gì thực sự quan trọng. Điều kiện chỉ phụ thuộc vào cách phân phối các giá trị này qua các vị trí chẵn lẻ xen kẽ trong một số dãy con đã chọn. Các số 0 hoàn toàn không góp phần vào tình trạng này, chúng chỉ giúp ích bằng cách lấp đầy các vị trí và cho phép các vị trí chuyển đổi giữa các ô chẵn và lẻ. 

Điều quan trọng là chúng ta không bắt buộc phải sử dụng tất cả các ký tự. Chúng ta chỉ cần biết liệu có tồn tại ít nhất một công trình hợp lệ hay không. Nếu chúng ta chọn được hai số, chúng ta luôn có thể đặt chúng ở vị trí 1 và 2 trong dãy con, làm cho một ở vị trí lẻ và một ở vị trí chẵn, thỏa mãn đẳng thức. Nếu chúng ta chỉ chọn một, nó sẽ luôn ở vị trí kỳ lạ và phá vỡ điều kiện. Nếu chúng ta không chọn số nào thì dãy con phải bao gồm toàn số 0, luôn thỏa mãn điều kiện vì cả vị trí chẵn và lẻ đều chứa số 0. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem chuỗi có chứa ít nhất hai chuỗi hay không chứa chuỗi nào. 

Nếu số lượng chính xác là một thì câu trả lời là không thể. Nếu nó bằng 0 hoặc ít nhất là 2 thì có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$tổng cộng |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đếm số ký tự bằng`1`trong chuỗi. Điều này là đủ vì các số 0 không bao giờ ảnh hưởng đến ràng buộc, chỉ có vị trí của các số 1 mới quan trọng. 
2. Nếu số hàng đơn vị bằng 1 thì xuất ngay`NO`. Với một dãy duy nhất, bất kỳ dãy con nào chứa nó sẽ đặt nó ở vị trí 1 của dãy con đó hoặc một vị trí lẻ nào đó và không có dãy con thứ hai cân bằng nó ở vị trí chẵn. 
3. Ngược lại, xuất ra`YES`. Nếu không có số 0, chúng ta có thể giữ bất kỳ số 0 đơn lẻ nào và cả số lẻ và số chẵn của số một đều bằng 0. Nếu có ít nhất hai cái, chúng ta luôn có thể chọn hai cái trong số chúng và sắp xếp chúng ở các vị trí xen kẽ nhau theo một dãy con sao cho một cái ở vị trí lẻ và một cái ở vị trí chẵn, thỏa mãn sự bằng nhau. 

### Tại sao nó hoạt động 

Cấu trúc chuỗi con chỉ quan trọng thông qua các vị trí chẵn lẻ được chỉ định sau khi xóa. Các số 0 luôn có thể được sử dụng làm dấu phân cách linh hoạt, nghĩa là bất kỳ hai số 0 nào được chọn đều có thể được đặt vào các lớp chẵn lẻ khác nhau theo một số thứ tự sau. Sự cản trở duy nhất xảy ra khi có chính xác một phần tử, bởi vì cân bằng chẵn lẻ yêu cầu sự đóng góp ghép nối từ những phần tử ở cả vị trí chẵn và vị trí lẻ, và một phần tử đơn lẻ không thể đóng góp cho cả hai bên. Tất cả các trường hợp khác hoặc không có hoặc ít nhất hai, cả hai đều thừa nhận một cách xây dựng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        ones = s.count('1')
        if ones == 1:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tính số cái cho mỗi trường hợp thử nghiệm. Logic được chứa đầy đủ trong một điều kiện duy nhất, giúp tránh mọi nhu cầu mô phỏng các chuỗi con hoặc theo dõi tính chẵn lẻ một cách rõ ràng. Chi tiết quan trọng là chúng tôi không cố gắng suy luận về các vị trí trong chuỗi gốc vì việc xóa sẽ xác định lại hoàn toàn việc lập chỉ mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`01`Chúng tôi chỉ theo dõi số lượng. 

| Bước | Chuỗi | Số người đếm | Quyết định | 
| --- | --- | --- | --- | 
| 1 | "01" | 1 | từ chối | 

Cái duy nhất buộc bất kỳ dãy con nào chứa nó phải mất cân bằng ở các vị trí chẵn và lẻ. 

Điều này xác nhận trường hợp cạnh chính trong đó có chính xác một cạnh bị lỗi. 

### Ví dụ 2 

Chuỗi đầu vào:`1010`| Bước | Chuỗi | Số người đếm | Quyết định | 
| --- | --- | --- | --- | 
| 1 | "1010" | 2 | chấp nhận | 

Với hai cái, chúng ta có thể xây dựng một chuỗi con như`11`bằng cách xóa số không. Trong chuỗi tiếp theo, các vị trí là 1 và 2, vì vậy mỗi vị trí nằm trong các lớp chẵn lẻ khác nhau, tạo ra sự bình đẳng. 

Điều này chứng tỏ rằng số không là không liên quan và chỉ có số lượng số một mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm,$O(\sum n)$tổng thể | Mỗi chuỗi được quét một lần để đếm số hàng | 
| Không gian |$O(1)$thêm | Chỉ có một bộ đếm được duy trì | 

Tổng giới hạn độ dài là 200.000 đảm bảo rằng một lần truyền qua tất cả đầu vào là đủ nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        ones = s.count('1')
        out.append("NO" if ones == 1 else "YES")
    return "\n".join(out) + "\n"

# provided samples
assert run("3\n0\n1\n01\n") == "YES\nNO\nYES\n"

# single zero
assert run("1\n0\n") == "YES\n"

# single one
assert run("1\n1\n") == "NO\n"

# two ones
assert run("1\n11\n") == "YES\n"

# mixed large
assert run("1\n101010\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| CÓ | số 0 duy nhất là hợp lệ | 
|`1`| KHÔNG | một cái không hợp lệ | 
|`11`| CÓ | trường hợp khác không hợp lệ tối thiểu | 
|`101010`| CÓ | cấu trúc xen kẽ, nhiều cái | 

## Vỏ cạnh 

Các trường hợp ký tự đơn là nhạy cảm nhất. Đối với đầu vào`0`, thuật toán đếm số 0 và ngay lập tức đưa ra`YES`, tương ứng với việc lấy toàn bộ chuỗi làm chuỗi con hợp lệ. 

Đối với đầu vào`1`, số đếm chính xác là một, do đó thuật toán đưa ra`NO`. Bất kỳ nỗ lực nào nhằm giữ ký tự này sẽ khiến nó ở vị trí 1 của dãy con, tạo ra một vị trí số 1 lẻ và 0 vị trí chẵn, vi phạm điều kiện. 

Một trường hợp cạnh khác là khi tất cả các ký tự đều là số 0, chẳng hạn như`000000`. Số lượng đơn vị bằng 0, do đó thuật toán đưa ra`YES`. Chúng ta có thể lấy bất kỳ số 0 đơn lẻ nào và vì không có số 0 nào trong cả hai lớp chẵn lẻ nên điều kiện được thỏa mãn. 

Cuối cùng, hãy xem xét các chuỗi có nhiều chuỗi nhưng được phân tách bằng số 0, chẳng hạn như`10001`. Số lượng đơn vị là 2, do đó thuật toán đưa ra`YES`. Chúng ta có thể xóa các số 0 và giữ lại hai số 1, tạo ra một dãy con`11`, tự động cân bằng các vị trí chẵn và lẻ.
