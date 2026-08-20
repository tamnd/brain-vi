---
title: "CF 102215A - Phòng và lối đi"
description: "Chúng ta có một dãy gồm (n+1) phòng và (n) lối đi. Đoạn (i) nối phòng (i-1) với phòng (i), nên việc di chuyển về phía đích luôn đồng nghĩa với việc xử lý mảng từ trái sang phải. Mỗi đoạn văn được mô tả bằng một số nguyên (ai). Giá trị tuyệt đối của nó là màu vượt qua."
date: "2026-08-20T02:40:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "A"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 415
verified: false
draft: false
---

[CF 102215A - Phòng và lối đi](https://codeforces.com/problemset/problem/102215/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 55 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy gồm (n+1) phòng và (n) lối đi. Đoạn (i) nối phòng (i-1) với phòng (i), nên việc di chuyển về phía đích luôn đồng nghĩa với việc xử lý mảng từ trái sang phải. Mỗi đoạn văn được mô tả bằng một số nguyên (a_i). Giá trị tuyệt đối của nó là màu vượt qua. Giá trị dương có nghĩa là đoạn văn sẽ kiểm tra màu đó trước khi cho phép chúng ta đi qua. Giá trị âm có nghĩa là lối đi luôn có thể được vượt qua, nhưng sau khi vượt qua nó, lối đi của màu đó trở nên không hợp lệ. Định dạng đầu vào và hai loại đoạn văn này được đưa ra bởi tuyên bố chính thức. 

Đối với mỗi (các) phòng bắt đầu, chúng tôi bắt đầu với mọi thẻ hợp lệ và liên tục vượt qua các đoạn (s+1,s+2,\ldots) cho đến khi một đoạn kiểm tra yêu cầu một thẻ đã không hợp lệ hoặc chúng tôi đạt đến phòng (n). Câu trả lời cho (s) là số lối đi vượt qua thành công, cũng là số phòng được vào khi di chuyển về phía phòng (n). 

Giới hạn (n\le 500000) loại trừ mọi thứ bậc hai. Một mô phỏng đơn giản cho mỗi phòng xuất phát có thể kiểm tra về 

[ 
n+(n-1)+\cdots+1=\frac{n(n+1)}2 
] 

đoạn trong trường hợp xấu nhất, đó là khoảng (1,25\cdot10^{11}) hoạt động khi (n=500000). Giới hạn hai giây yêu cầu một giải pháp tuyến tính cơ bản hoặc nhiều nhất là một giải pháp rất gần với nó. Thực tế là mọi màu vượt qua nằm trong khoảng từ (1) đến (n) cũng cho phép chúng ta lưu trữ thông tin về từng màu trong các mảng thông thường thay vì sử dụng các cấu trúc đa năng đắt tiền. 

Có một số trường hợp ranh giới có thể đánh lừa việc triển khai trực tiếp. Với (n=1) và đầu vào`1`, câu trả lời là`1`, bởi vì lối đi duy nhất có thể được vượt qua. Việc triển khai giả định mọi câu trả lời đều cần một đoạn sau có thể tạo ra từng lỗi một. 

Coi như```
2
-1 1
```Câu trả lời là`1 1`. Bắt đầu từ phòng (0), đoạn 1 bị gạch chéo và vô hiệu hóa màu 1. Đoạn 2 sau đó từ chối chúng tôi nên chỉ có một đoạn văn bị gạch chéo. Bắt đầu từ phòng (1), chúng ta chỉ gặp lối đi số 2 và có thể băng qua nó. Giải pháp coi lối đi phủ định là chặn ngay lập tức là sai, bởi vì lối đi phủ định không bao giờ từ chối lối vào. 

Thứ tự ngược lại cũng rất quan trọng:```
2
1 -1
```Câu trả lời là`2 1`. Bắt đầu từ phòng (0), đoạn dương được gạch chéo trong khi đoạn văn của nó vẫn hợp lệ và đoạn âm sau đó cũng bị gạch chéo. Một giải pháp tìm kiếm bất kỳ sự xuất hiện tiêu cực nào của cùng một màu ở bất kỳ đâu trong mảng có thể từ chối đoạn đầu tiên một cách không chính xác. Chỉ một trường hợp tiêu cực đã bị vượt qua mới có thể làm mất hiệu lực của thẻ. 

Cuối cùng, việc vô hiệu chỉ quan trọng sau phòng bắt đầu đã chọn. Vì```
2
-1 1
```bắt đầu từ phòng (1) đưa ra câu trả lời`1`, mặc dù có một đoạn màu âm-1 ở bên trái. Mọi vị trí bắt đầu đều bắt đầu với tất cả các thẻ đều hợp lệ, vì vậy các sự kiện trước khi bắt đầu không được ảnh hưởng đến truy vấn đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force tuân theo quy trình theo đúng nghĩa đen. Đối với mỗi phòng bắt đầu, hãy tạo trạng thái mô tả những màu nào vẫn hợp lệ, quét các đoạn bên phải, vượt qua một đoạn phủ định và vô hiệu hóa màu của nó, đồng thời dừng lại ở đoạn tích cực đầu tiên có màu đã bị vô hiệu. Điều này đúng vì nó tái hiện chính xác các quy luật chuyển động. 

Vấn đề là việc quét lặp đi lặp lại. Nếu mọi truy vấn có thể đến cuối, truy vấn đầu tiên sẽ kiểm tra (n) đoạn văn, truy vấn thứ hai sẽ kiểm tra (n-1), v.v. Tổng số là (n(n+1)/2), đạt khoảng (1,25\cdot10^{11}) lượt truy cập qua đường cho (n=500000). Điều đó vượt xa giới hạn thời gian. 

Sự quan sát hữu ích đến từ việc đảo ngược hướng suy nghĩ. Giả sử chúng ta hiện đang xem xét đoạn (i) trong khi quét từ phải sang trái. Một đoạn màu âm (c) cuối cùng chỉ có thể gây ra lỗi nếu có một đoạn màu dương (c) ở đâu đó bên phải nó. Trong số tất cả các lối đi tích cực như vậy, chỉ có lối đi gần nhất quan trọng đối với lối đi tiêu cực cụ thể đó, bởi vì đây là nơi đầu tiên mà du khách sẽ dừng lại sau khi vô hiệu hóa đường đi. 

Trong khi quét từ phải sang trái, chúng ta có thể giữ`next_pos[c]`, đoạn màu dương gần nhất (c) hiện được biết ở bên phải. Khi chúng ta gặp một đoạn màu âm (c),`next_pos[c]`cho chúng ta biết đoạn văn sớm nhất mà đoạn văn tiêu cực này có thể gây ra điểm dừng. Sau đó chúng ta có thể duy trì một ranh giới toàn cầu,`limit`, bằng vị trí dừng sớm nhất gây ra bởi bất kỳ đoạn âm nào đã được xử lý. 

Đây là nén khóa. Thay vì mô phỏng từng vị trí bắt đầu một cách riêng biệt, hậu tố ở bên phải của đoạn hiện tại được tóm tắt chỉ bằng hai loại thông tin: đoạn tích cực gần nhất cho mỗi màu và vị trí thất bại sớm nhất do bất kỳ đoạn tiêu cực liên quan nào gây ra. Phép truy toán ngược được sử dụng ở đây cũng được phản ánh trong các giải pháp hiện có cho vấn đề này. 

Nếu đoạn (i) là khẳng định thì nó luôn có thể được gạch bỏ khi (i) là đoạn đầu tiên của truy vấn, bởi vì chưa có đoạn phủ định nào ở bên phải của nó được gạch bỏ. Câu trả lời của nó chỉ đơn giản là nhiều hơn một câu trả lời cho đoạn văn (i+1). 

Nếu đoạn (i) là âm và màu của nó không có đoạn dương ở bên phải thì việc vượt qua nó không thể tạo ra thất bại trong tương lai, do đó, một lần nữa câu trả lời của nó nhiều hơn câu trả lời cho (i+1). Nếu một đoạn tích cực cùng màu tồn tại ở vị trí (p), thì bắt đầu từ (i) cuối cùng sẽ thất bại ở hoặc trước (p). Chúng tôi cập nhật ranh giới toàn cầu với (p-1), bởi vì khách du lịch chỉ có thể vượt qua thành công các lối đi qua (p-1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Quét ngược tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các đoạn văn bằng cách sử dụng các chỉ mục dựa trên một. Đoạn (i) tương ứng với truy vấn có phòng bắt đầu là (i-1), do đó việc tính toán câu trả lời cho mỗi đoạn trực tiếp sẽ đưa ra thứ tự đầu ra được yêu cầu. 
2. Tạo`next_pos[c]`, ban đầu bằng 0, cho mọi màu vượt qua. Trong quá trình quét từ phải sang trái,`next_pos[c]`sẽ chứa đoạn màu dương gần nhất (c) ở bên phải của vị trí hiện tại. 
3. Tạo`ans[i]`cho mỗi đoạn văn và khởi tạo`ans[n+1]`về không. Vị trí giả định (n+1) thể hiện không còn đoạn văn nào, do đó nó đưa ra một trường hợp cơ sở rõ ràng. 
4. Duy trì`limit = n`. Biến này đại diện cho đoạn cuối cùng vẫn có thể được vượt qua trước khi một số đoạn tiêu cực đã thấy gây ra lỗi. Nếu không có sự cố nào như vậy tồn tại, giá trị (n) có nghĩa là khách du lịch có thể đến đích. 
5. Quét (i=n,n-1,\ldots,1). Nếu (a_i>0), bản thân đoạn (i) an toàn khi bắt đầu ở đó, vì vậy hãy đặt 

[ 
ans[i]=ans[i+1]+1. 
] 

thiết lập sau đó`next_pos[a_i] = i`. Bởi vì chúng tôi đang quét từ phải sang trái, nhiệm vụ này ghi lại lần xuất hiện tích cực gần nhất của màu này. 

1. Nếu (a_i<0), đặt (c=-a_i). Nếu như`next_pos[c]`bằng 0, không có đường chuyển tích cực nào của màu này sang bên phải. Vượt qua lối đi tiêu cực hiện tại không thể gây ra thất bại trong tương lai, vì vậy hãy đặt 
[ 
ansi]=ansi+1]+1. 
]
Nếu như`next_pos[c]=p`, khi đó việc vượt qua đoạn (i) sẽ làm mất hiệu lực của màu (c) và đoạn dương tại (p) sẽ là nơi đầu tiên có thể xảy ra khi thẻ không hợp lệ đó bị từ chối. cập nhật 

[ 
giới hạn=\min(giới hạn,p-1). 
] 

Du khách bắt đầu từ (i) sau đó có thể đi qua chính xác các đoạn từ (i) đến`limit`, cho 
[ 
và [i]=giới hạn-i+1. 
] 
Lý do duy nhất`limit`Chỉ cần người du hành dừng lại ở điểm thất bại sớm nhất trong số tất cả các đoạn phủ định ở hậu tố là đủ. Việc đạt mức tối thiểu trên các vị trí dừng của họ sẽ nắm bắt chính xác thất bại đầu tiên đó. 

1. Cuối cùng, in`ans[1], ans[2], ..., ans[n]`. Trả lời`ans[i]`tương ứng với việc bắt đầu từ phòng (i-1), khớp chính xác với các phòng bắt đầu được yêu cầu (0) đến (n-1). 

Tại sao nó hoạt động: sau khi xử lý các vị trí ở bên phải của (i),`next_pos[c]`là đoạn màu dương gần nhất (c) trong hậu tố đó. Mọi đoạn tiêu cực đã được xử lý sẽ không có đoạn tích cực phù hợp sau này hoặc đã xác định được đoạn chặn sớm nhất có thể của nó. Như vậy`limit`là ranh giới chặn sớm nhất được tạo ra bởi bất kỳ đoạn phủ định nào trong hậu tố được xử lý. Khi chúng tôi thêm đoạn văn (i), một đoạn văn khẳng định luôn có thể được duyệt qua khi bắt đầu truy vấn, trong khi một đoạn văn phủ định không tạo ra hạn chế mới hoặc giới thiệu đoạn văn khẳng định phù hợp của nó như một ứng cử viên khác cho hạn chế sớm nhất. Do đó, bất biến đưa ra chính xác đoạn đầu tiên có thể dừng mọi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    next_pos = [0] * (n + 1)
    ans = [0] * (n + 2)

    limit = n

    for i in range(n, 0, -1):
        x = a[i]

        if x > 0:
            ans[i] = ans[i + 1] + 1
            next_pos[x] = i
        else:
            color = -x
            p = next_pos[color]

            if p == 0:
                ans[i] = ans[i + 1] + 1
            else:
                limit = min(limit, p - 1)
                ans[i] = limit - i + 1

    print(*ans[1:n + 1])

if __name__ == "__main__":
    solve()
```Mảng đầu vào được tạo dựa trên một bằng cách chèn số 0 giả vào chỉ số 0. Điều đó giữ cho số đoạn văn (i) phù hợp với phép truy toán toán học và tránh việc dịch lặp đi lặp lại giữa các chỉ số đoạn văn và chỉ số phòng.`next_pos`được lập chỉ mục theo màu sắc. Vì mỗi màu có nhiều nhất là (n), nên một danh sách có độ dài (n+1) là đủ và nhanh hơn cũng như tiết kiệm bộ nhớ hơn so với từ điển cho vấn đề này.`ans[n+1]`vẫn bằng 0, điều này mang lại sự truy hồi cho đoạn cuối cùng là trường hợp cơ sở tự nhiên của nó. Ví dụ: nếu đoạn văn cuối cùng là tích cực,`ans[n] = ans[n+1] + 1 = 1`. 

Thứ tự các hoạt động cho một lối đi tích cực rất quan trọng. Chúng tôi tính toán câu trả lời của nó trước khi lưu trữ vị trí của nó trong`next_pos`. Đoạn văn tích cực không thể bị chặn bởi đoạn văn phủ định ở bên phải của nó khi truy vấn bắt đầu chính xác tại đoạn văn này, vì vậy nó không được vô tình trở thành một phần thông tin được sử dụng để xác định câu trả lời của chính nó. 

Đối với một đoạn văn tiêu cực,`next_pos[color]`chỉ chứa các đoạn tích cực ở bên phải của nó, bởi vì đó là những vị trí đã được quét ngược lại. Đó chính xác là tập hợp các đoạn văn có thể trở thành đoạn văn chặn sau khi đoạn văn tiêu cực này bị vượt qua. 

biểu hiện`limit - i + 1`đếm các đoạn văn một cách toàn diện. Nếu đoạn bị chặn đầu tiên là (p), thì`limit = p - 1`, và các đoạn thành công là (i,i+1,\ldots,p-1). Số lượng của chúng là (p-i), giống như`limit-i+1`. 

Số nguyên Python không bị tràn, vì vậy mối quan tâm thực tế duy nhất là phân bổ bộ nhớ tuyến tính và tốc độ đầu vào. Việc thực hiện sử dụng`sys.stdin.readline`và một số lượng nhỏ mảng, cả hai đều phù hợp với (n=500000). 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
6
1 -1 -1 1 -1 1
```chúng tôi xử lý các đoạn từ phải sang trái. Bảng hiển thị trạng thái liên quan sau khi xử lý từng đoạn. 

| (i) | (a_i) |`next_pos[|a_i|]`trước |`limit`sau |`ans[i]`| 
|---:|---:|---:|---:|---:| 
| 6 | 1 | 0 | 6 | 1 | 
| 5 | -1 | 6 | 5 | 1 | 
| 4 | 1 | 6 | 5 | 2 | 
| 3 | -1 | 4 | 3 | 1 | 
| 2 | -1 | 4 | 3 | 2 | 
| 1 | 1 | 4 | 3 | 3 | 

Ở đoạn 6, màu 1 không xuất hiện tích cực ở bên phải của nó, do đó bắt đầu từ đó sẽ có một bước. Sau khi ghi đoạn 6, đoạn 5 coi đó là đoạn dương-1 có màu dương gần nhất và thiết lập ranh giới dừng ở đoạn 5. Đoạn 4 tự nó là dương và có thể bị vượt qua, trong khi đoạn 3 âm sẽ tìm đoạn 4 dương gần hơn và di chuyển ranh giới toàn cục sang đoạn 3. Sau đó, hai đoạn còn lại được xử lý bằng ranh giới đó. 

Các câu trả lời kết quả là`3 2 1 2 1 1`. Ví dụ: bắt đầu từ phòng 0 có nghĩa là vượt qua các đoạn 1, 2 và 3, sau đó đoạn 4 kiểm tra màu 1 đã bị đoạn 2 vô hiệu. 

Đối với mẫu 2,```
7
2 -1 -2 -3 1 3 2
```quá trình quét ngược hoạt động như sau. 

| (i) | (a_i) | Liên quan`next_pos`trước |`limit`sau |`ans[i]`| 
| --- | --- | --- | --- | --- | 
| 7 | 2 |`next_pos[2]=0`| 7 | 1 | 
| 6 | 3 |`next_pos[3]=0`| 7 | 2 | 
| 5 | 1 |`next_pos[1]=0`| 7 | 3 | 
| 4 | -3 |`next_pos[3]=6`| 5 | 2 | 
| 3 | -2 |`next_pos[2]=7`| 5 | 3 | 
| 2 | -1 |`next_pos[1]=5`| 4 | 3 | 
| 1 | 2 |`next_pos[2]=7`| 4 | 4 | 

Ở đoạn 4, đoạn 3 màu âm nhìn thấy màu dương 3 ở đoạn 6, do đó bắt đầu từ đó không thể vượt qua đoạn 6. Điều đó mang lại`limit = 5`. Đoạn 3 giới thiệu một lỗi khác có thể xảy ra ở đoạn 7, muộn hơn và không thay đổi giới hạn. Đoạn 2 giới thiệu một lỗi ở đoạn 5, điều này cải thiện giới hạn lên 4. 

Đầu ra cuối cùng là`4 3 3 2 3 2 1`, phù hợp với mẫu Dấu vết này cho thấy tại sao ranh giới phải ở mức tối thiểu trên tất cả các đoạn phủ định có liên quan thay vì chỉ đơn giản là vị trí chặn liên kết với đoạn hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi đoạn được xử lý chính xác một lần, với các phép toán ranh giới và màu sắc không đổi theo thời gian. | 
| Không gian | (O(n)) | Mảng đoạn văn, mảng câu trả lời và mảng vị trí gần nhất theo màu đều sử dụng không gian tuyến tính. | 

Với (n\le500000), thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi đoạn, do đó tổng số thao tác tỷ lệ thuận với kích thước đầu vào. Ba mảng chính có kích thước tuyến tính, nằm trong giới hạn bộ nhớ 256 MB để triển khai Python này. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve_data(data: str) -> str:
    input = io.StringIO(data).readline

    n = int(input())
    a = [0] + list(map(int, input().split()))

    next_pos = [0] * (n + 1)
    ans = [0] * (n + 2)

    limit = n

    for i in range(n, 0, -1):
        x = a[i]

        if x > 0:
            ans[i] = ans[i + 1] + 1
            next_pos[x] = i
        else:
            color = -x
            p = next_pos[color]

            if p == 0:
                ans[i] = ans[i + 1] + 1
            else:
                limit = min(limit, p - 1)
                ans[i] = limit - i + 1

    return " ".join(map(str, ans[1:n + 1]))

def run(inp: str) -> str:
    return solve_data(inp).strip()

assert run("""6
1 -1 -1 1 -1 1
""") == "3 2 1 2 1 1", "sample 1"

assert run("""7
2 -1 -2 -3 1 3 2
""") == "4 3 3 2 3 2 1", "sample 2"

assert run("""1
1
""") == "1", "minimum-size input"

assert run("""2
-1 1
""") == "1 1", "negative passage invalidates the following positive passage"

assert run("""4
1 -1 -1 1
""") == "3 2 1 1", "repeated negative occurrences of one color"

n = 500000
maximum_case = str(n) + "\n" + " ".join(["1"] * n) + "\n"
expected = " ".join(map(str, range(n, 0, -1)))
assert run(maximum_case) == expected, "maximum-size all-positive input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Kích thước tối thiểu và ranh giới lối đi cuối cùng | 
|`2 / -1 1`|`1 1`| Một đoạn phủ định chỉ làm mất hiệu lực màu của nó sau khi nó bị gạch chéo | 
|`4 / 1 -1 -1 1`|`3 2 1 1`| Sự vô hiệu lặp đi lặp lại của cùng một màu và ranh giới dừng sớm nhất | 
|`500000 / 1 1 ... 1`|`500000 499999 ... 1`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1
1
```quá trình quét ngược bắt đầu bằng`limit = 1`. Đoạn 1 là tích cực, vì vậy`ans[1] = ans[2] + 1 = 1`. Vị trí của màu 1 sau đó được ghi là 1. Đầu ra là`1`, điều đó có nghĩa chính xác là lối đi duy nhất có sẵn có thể được vượt qua. 

Đối với một đoạn tiêu cực có một đoạn tích cực sau đó,```
2
-1 1
```đầu tiên quá trình quét nhìn thấy đoạn 2, ghi lại màu dương 1 và nhận được`ans[2]=1`. Ở đoạn 1,`next_pos[1]=2`, do đó đoạn phủ định làm mất hiệu lực màu 1 và đặt`limit=1`. Câu trả lời trở thành`1-1+1=1`. Đầu ra là`1 1`. Điều này mắc phải sai lầm phổ biến là coi bản thân một đoạn văn tiêu cực là không thể vượt qua được. 

Đối với một đoạn tích cực theo sau là một đoạn tiêu cực,```
2
1 -1
```đoạn 2 là đoạn âm và không có đoạn màu dương-1 ở bên phải của nó, vì vậy`ans[2]=1`. Đoạn 1 là dương và do đó có thể vượt qua ngay lập tức, cho`ans[1]=2`. Đầu ra là`2 1`. Đoạn văn phủ định không làm mất hiệu lực hồi tố của đoạn văn tích cực trước đó. 

Đối với những lần xảy ra tiêu cực lặp đi lặp lại,```
4
1 -1 -1 1
```quét ngược ghi lại đoạn màu dương-1 ở vị trí 4. Đoạn âm ở vị trí 3 được đặt`limit=3`, trong khi đoạn phủ định ở vị trí 2 giữ nguyên ranh giới vì đoạn khẳng định phù hợp của nó vẫn ở vị trí 4. Do đó, câu trả lời là`3 2 1 1`. Bắt đầu từ phòng 0, khách du lịch đi qua các đoạn 1, 2 và 3, nhưng đoạn 4 từ chối thẻ màu-1 hiện không hợp lệ. Bắt đầu từ phòng 1 hoặc phòng 2, bạn sẽ đi bộ được quãng đường ngắn dần. 

Đối với trường hợp kích thước tối đa bao gồm toàn bộ các đoạn dương,```
500000
1 1 1 ... 1
```không bao giờ có một đoạn tiêu cực làm mất hiệu lực bất kỳ đường chuyền nào. Sự tái phát ngược lại chỉ đơn giản là cho`ans[i] = ans[i+1] + 1`, sản xuất`500000,499999,\ldots,1`. Quá trình quét thực hiện chính xác (500000) lần lặp, chứng minh tại sao nghiệm tuyến tính phù hợp với ràng buộc trong khi mô phỏng bậc hai thì không.
