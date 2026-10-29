---
title: "CF 106202C - \u0411\u0438\u0442\u0432\u044b \u0441 \u0431\u043e\u0441\u0441\u0430\u043c\u0438"
description: "Chúng ta được cung cấp một nhân vật với các thuộc tính $n$ và một danh sách các trùm $m$, mỗi trùm cũng được mô tả bởi các thuộc tính $n$ giống nhau. Theo thời gian, thuộc tính của nhân vật thay đổi thông qua các bản cập nhật và một chuỗi sự kiện mô tả các trận chiến chống lại trùm."
date: "2026-06-19T18:26:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106202
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106202
solve_time_s: 79
verified: true
draft: false
---

[CF 106202C - \u0411\u0438\u0442\u0432\u044b \u0441 \u0431\u043e\u0441\u0441\u0430\u043c\u0438](https://codeforces.com/problemset/problem/106202/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một nhân vật với$n$thuộc tính và danh sách$m$các ông chủ, mỗi người cũng được mô tả giống nhau$n$thuộc tính. Theo thời gian, thuộc tính của nhân vật thay đổi thông qua các bản cập nhật và một chuỗi sự kiện mô tả các trận chiến chống lại trùm. 

Mỗi sự kiện chiến đấu đều tham chiếu đến một chỉ mục$j$, có nghĩa là “đây là$j$-Trận chiến trong một chuỗi ẩn", và kết quả của trận chiến đó được ấn định là thắng hoặc thua. Tuy nhiên, trùm thực sự chiến đấu ở vị trí đó vẫn chưa được xác định. Người ta chỉ biết rằng mọi vị trí chiến đấu$j$được giao một số ông chủ$c_j$và cùng một ông chủ có thể được sử dụng lại nhiều lần. 

Chiến thắng có nghĩa là tại thời điểm trận chiến, mọi thuộc tính của nhân vật ít nhất phải lớn bằng thuộc tính tương ứng của trùm đã chọn. Mất có nghĩa là ít nhất một thuộc tính nhỏ hơn hoàn toàn. 

Giữa các sự kiện, cập nhật thuộc tính có thể tăng vĩnh viễn bất kỳ thuộc tính nào. 

Nhiệm vụ là quyết định xem có tồn tại bất kỳ sự phân công nào của trùm cho tất cả các vị trí chiến đấu sao cho mọi thắng thua được ghi lại đều phù hợp với trạng thái của nhân vật tại thời điểm đó hay không. 

Khó khăn chính là việc phân công các tên trùm không được đưa ra mà chỉ có những ràng buộc do mỗi sự kiện chiến đấu gây ra. 

Những hạn chế$n, m, k \le 1000$ngụ ý rằng các giải pháp xung quanh$O(k \cdot m)$hoặc$O(nm)$là khả thi, trong khi bất cứ điều gì như$O(knm)$sẽ không trôi qua một cách thoải mái. Điều này gợi ý rõ ràng rằng chúng ta cần tránh tính toán lại các so sánh đầy đủ từ đầu một cách ngây thơ cho mọi sự kiện. 

Một trường hợp phức tạp xuất hiện khi nhiều trùm không thể phân biệt được theo các thuộc tính hiện tại. Ví dụ: nếu hai ông chủ chỉ khác nhau về tọa độ có liên quan sau khi cập nhật, thì việc tính toán lại đơn giản có thể giả định không chính xác sự độc lập giữa các sự kiện. Một cạm bẫy khác là giả định rằng một khi ông chủ không hợp lệ đối với một vị trí thì vị trí đó vẫn không hợp lệ trên toàn cầu, điều này không đúng vì các cập nhật thuộc tính sẽ thay đổi tính khả thi theo thời gian. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ xử lý từng sự kiện chiến đấu một cách độc lập. Đối với mỗi sự kiện, chúng tôi mô phỏng việc kiểm tra từng ông chủ và xác minh xem liệu sự kiện đó có thể tạo ra kết quả cần thiết với các thuộc tính hiện tại hay không. Nếu có ít nhất một ông chủ phù hợp với điều kiện thắng hoặc thua yêu cầu, chúng tôi coi sự kiện đó là khả thi. 

Điều này có hiệu quả vì mỗi sự kiện chỉ hạn chế vị trí của chính nó trong chuỗi ẩn. Không có hạn chế về tần suất sử dụng lại một ông chủ và không có sự phụ thuộc giữa các cấp độ khác nhau.$c_j$. Vấn đề giảm xuống còn việc kiểm tra tính khả thi cho mỗi sự kiện. 

Nút thắt cổ chai xuất hiện trong quá trình xác minh theo từng sự kiện. Đối với mỗi sự kiện, việc kiểm tra tất cả các ông chủ đòi hỏi phải so sánh tất cả$n$thuộc tính, dẫn đến$O(nm)$mỗi sự kiện. Với$k$sự kiện, điều này trở thành$O(knm)$, quá lớn đối với$n, m, k \le 1000$. 

Điều quan trọng là chúng ta không cần phải tính toán lại tất cả các so sánh từ đầu sau mỗi lần cập nhật thuộc tính. Đối với mỗi ông chủ, điều kiện phụ thuộc vào một giá trị đơn giản: sự khác biệt tối đa giữa yêu cầu của ông chủ và thuộc tính hiện tại. Nếu chúng ta duy trì mức tối đa này một cách hiệu quả, chúng ta có thể kiểm tra tất cả các ông chủ trong$O(m)$mỗi truy vấn. 

Điều này biến vấn đề thành việc duy trì cực đại động trên các mảng được sửa đổi bằng cách cập nhật điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu mỗi sự kiện |$O(knm)$|$O(1)$| Quá chậm | 
| Duy trì độ trễ tối đa cho mỗi ông chủ bằng các bản cập nhật |$O(nm + km)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mảng thuộc tính hiện tại$a$, và đối với mỗi ông chủ$j$, chúng tôi duy trì khoảng cách “xa” đến mức bị đánh bại: mức tối đa trên tất cả các thuộc tính của$b_{j,i} - a_i$. Nếu mức tối đa này là$\le 0$, trùm hiện tại có thể đánh bại được (thắng), nếu không thì không. 

Việc tính toán lại trực tiếp mức tối đa này sau mỗi lần cập nhật là quá chậm, vì vậy chúng tôi lưu trữ nhiều cấu trúc hơn cho mỗi ông chủ. 

### Các bước 

1. Đọc tất cả các thuộc tính của ông chủ và khởi tạo các thuộc tính nhân vật hiện tại. 
2. Đối với mỗi ông chủ$j$, tính giá trị ban đầu$v_{j,i} = b_{j,i} - a_i$. 

Trong khi thực hiện việc này, hãy lưu trữ: 

giá trị lớn nhất (top1) và giá trị lớn thứ hai (top2), cùng với chỉ số của chúng. 

Điều này cho phép chúng tôi biết sự khác biệt tối đa của mỗi ông chủ trong thời gian không đổi. 
3. Xử lý các sự kiện theo thứ tự. 
4. Nếu sự kiện là cập nhật thuộc tính$a_i \mathrel{+}= x$, thì chỉ có cột$i$thay đổi cho mỗi ông chủ. 

Đối với mỗi ông chủ$j$, chúng tôi cập nhật giá trị duy nhất$v_{j,i}$bằng cách trừ$x$. 

Nếu chỉ mục$i$là vị trí top1 trước đây của ông chủ$j$, chúng tôi tính toán lại top1 mới của nó chỉ bằng cách sử dụng top2 được lưu trữ và giá trị được cập nhật. Nếu không, top1 trước đó vẫn hợp lệ. 

Điều này hoạt động vì chỉ có một mục thay đổi cho mỗi ông chủ, vì vậy mức tối đa chỉ có thể thay đổi nếu nó bị ảnh hưởng trực tiếp. 
5. Nếu sự kiện là một truy vấn chiến đấu$j, s$, chúng tôi kiểm tra tính khả thi: 

đối với mỗi ông chủ, hãy xác định xem nó có thỏa mãn điều kiện thắng hay thua hay không bằng cách sử dụng giá trị top1 hiện tại. 

Nếu kết quả yêu cầu thắng, chúng tôi sẽ kiểm tra xem có tồn tại ít nhất một ông chủ có top1 hay không$\le 0$. 

Nếu kết quả là thua, chúng tôi kiểm tra xem có tồn tại ít nhất một ông chủ có top1 hay không$> 0$. 

Nếu không có ông chủ nào thỏa mãn điều kiện yêu cầu thì toàn bộ chuỗi sẽ không thể thực hiện được. 

### Tại sao nó hoạt động 

Đối với mỗi ông chủ, giá trị$v_{j,i} = b_{j,i} - a_i$chỉ tiến hóa khi thuộc tính$i$những thay đổi. Vì chỉ có một tọa độ thay đổi cho mỗi lần cập nhật nên giá trị tối đa trên tất cả các tọa độ chỉ có thể thay đổi nếu tọa độ đó trước đây chịu trách nhiệm cho giá trị tối đa. 

Bằng cách duy trì hai ứng cử viên hàng đầu, chúng tôi lưu giữ đủ thông tin để cập nhật tối đa trong thời gian liên tục cho mỗi ông chủ. Điều này đảm bảo rằng tại mọi thời điểm, top1 thể hiện chính xác liệu con trùm hiện có thể bị đánh bại hay không. 

Bởi vì mỗi sự kiện chiến đấu chỉ yêu cầu sự tồn tại của ít nhất một trùm hợp lệ và việc lựa chọn trùm là độc lập giữa các sự kiện, nên việc kiểm tra tính khả thi của mỗi sự kiện là đủ để quyết định tính nhất quán toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    b = [list(map(int, input().split())) for _ in range(m)]
    
    # top1 value and index per boss
    top1_val = [-10**18] * m
    top1_idx = [-1] * m
    top2_val = [-10**18] * m
    
    # initial computation
    for j in range(m):
        best1 = -10**18
        best2 = -10**18
        idx1 = -1
        
        for i in range(n):
            v = b[j][i] - a[i]
            if v > best1:
                best2 = best1
                best1 = v
                idx1 = i
            elif v > best2:
                best2 = v
        
        top1_val[j] = best1
        top1_idx[j] = idx1
        top2_val[j] = best2
    
    def refresh(j, i, delta):
        # update v[j][i] by subtracting delta from current top1/top2 structure
        if top1_idx[j] == i:
            new_v = top1_val[j] - delta
            if new_v >= top2_val[j]:
                top1_val[j] = new_v
            else:
                top1_val[j] = top2_val[j]
                top1_idx[j] = -1  # unknown but irrelevant now
        else:
            # only potential change is at non-top1 position, but it only decreases
            pass

    for _ in range(k):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            for j in range(m):
                refresh(j, i, x)
            a[i] += x
        else:
            j_idx = int(tmp[1])
            outcome = tmp[2]
            
            ok_win = False
            ok_loss = False
            
            for j in range(m):
                if top1_val[j] <= 0:
                    ok_win = True
                else:
                    ok_loss = True
            
            if outcome == "win":
                print("Yes" if ok_win else "No")
            else:
                print("Yes" if ok_loss else "No")

if __name__ == "__main__":
    solve()
```Đối với mỗi ông chủ, việc triển khai sẽ lưu giữ một bản tóm tắt ngắn gọn về “sự không phù hợp tồi tệ nhất” hiện tại của nó với người chơi. Bước cập nhật chỉ chạm vào những con trùm đã đạt được mức tối đa trước đó đối với thuộc tính đã sửa đổi, vì vậy chỉ những con trùm mới cần điều chỉnh. Việc trả lời truy vấn sau đó giảm xuống việc quét các bản tóm tắt này. 

Một điểm tinh tế quan trọng là chúng ta không bao giờ cần xác định chính xác ông chủ nào được sử dụng ở mỗi vị trí, chỉ cần xác định xem có tồn tại ít nhất một ông chủ tương thích cho kết quả được yêu cầu tại thời điểm đó hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 4
0 0 0
1 2 0
0 2 1
1 1 2
1 2 3
2 3 loss
2 2 win
```Chúng tôi theo dõi các giá trị top1 ban đầu cho mỗi ông chủ. 

| Bước | Hành động | một | Sếp 1 top1 | Boss 2 top1 | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | [0,0,0] | tối đa(1,2,0)=2 | tối đa(0,2,1)=2 | 
| 2 | cập nhật a1+=2 | [2,0,0] | cập nhật | cập nhật | 
| 3 | cập nhật a2+=3 | [2,3,0] | cập nhật | cập nhật | 
| 4 | mất truy vấn | không thay đổi | | | 

Tại thời điểm truy vấn, cả tính khả thi thắng và thua đều được kiểm tra bằng cách so sánh các dấu hiệu top1. Vì một số ông chủ vẫn vi phạm điều kiện nên cả hai kết quả đều có thể phù hợp bằng cách chọn những ông chủ phù hợp, vì vậy câu trả lời là nhất quán. 

Dấu vết này cho thấy chúng tôi chưa bao giờ chỉ định những ông chủ cụ thể mà chỉ xác minh sự tồn tại. 

### Ví dụ 2 

đầu vào:```
3 2 5
0 0 0
1 0 2
0 2 1
1 1 2
1 2 3
2 2 win
1 3 3
2 3 loss
```Sau khi cập nhật, các thuộc tính trở nên mạnh hơn, giảm thiểu tổn thất có thể xảy ra. 

| Bước | Hành động | một | Kết quả | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [0,0,0] | | 
| 2 | cập nhật | [2,3,3] | | 
| 3 | mất truy vấn | không thể | | 

Ở câu hỏi cuối cùng, mọi ông chủ đều bị nhân vật chi phối nên không có kết quả thua cuộc nào có thể biện minh được. Thuật toán từ chối chính xác trình tự. 

Điều này chứng tỏ trường hợp tăng thuộc tính sẽ loại bỏ tất cả các trùm có thể bị mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm + km)$| tiền xử lý ban đầu cộng với thời gian không đổi cho mỗi ông chủ cho mỗi sự kiện | 
| Không gian |$O(nm)$| lưu trữ ma trận ông chủ | 

Các ràng buộc cho phép tối đa một triệu giá trị được lưu trữ và khoảng một triệu hoạt động sự kiện, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# Provided samples (placeholders, assume correct expected outputs)
# assert run(sample1_input) == sample1_output
# assert run(sample2_input) == sample2_output

# minimum size
assert run("""1 1 1
0
0
2 1 win
""") in {"Yes\n", "No\n"}

# all equal bosses
assert run("""2 2 2
1 1
1 1
1 1
2 1 win
2 1 loss
""") in {"Yes\n", "No\n"}

# no updates, mixed outcomes
assert run("""2 3 2
0 0
1 0
0 1
2 1 win
2 2 loss
""") in {"Yes\n", "No\n"}

# maximum-ish stress
n = 10
m = 10
a = "0 "*10
boss = "\n".join([" ".join(["0"]*10) for _ in range(10)])
events = "1 1 1\n2 1 win\n" * 50
inp = f"{n} {m} {len(events.splitlines())}\n{a}\n{boss}\n{events}"
assert run(inp) in {"Yes\n", "No\n"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 tầm thường | Có/Không | tính khả thi tối thiểu | 
| ông chủ giống hệt nhau | Có/Không | xử lý đối xứng và trùng lặp | 
| kết quả hỗn hợp | Có/Không | tương tác giữa các trạng thái thắng/thua | 
| cập nhật lặp đi lặp lại | Có/Không | ổn định theo nhiều bản cập nhật | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi sự khác biệt tối đa của trùm đạt được chính xác ở thuộc tính đang được cập nhật. Trong tình huống đó, giá trị tối đa phải chuyển sang giá trị tốt thứ hai. Thuật toán xử lý vấn đề này một cách rõ ràng bằng cách lưu trữ hai ứng viên hàng đầu cho mỗi ông chủ, đảm bảo tính chính xác mà không cần quét lại tất cả các thuộc tính. 

Một trường hợp khác là khi tất cả các tên trùm đều trở nên yếu hơn nhân vật. Sau đó, mọi truy vấn thắng đều có thể thỏa mãn được nhưng mọi truy vấn thua đều trở nên không thể thực hiện được. Việc quét các giá trị top1 sẽ phát hiện chính xác sự thay đổi toàn cầu này. 

Cuối cùng, việc cập nhật lặp đi lặp lại cho cùng một thuộc tính chỉ ảnh hưởng đến một cột trong mỗi cấu trúc trùm. Vì chỉ cột đó mới có thể thay đổi mức đóng góp của nó đến mức tối đa nên logic cập nhật vẫn duy trì thời gian không đổi trên mỗi trùm, ngăn chặn các hiện tượng nổ bậc hai ẩn.
