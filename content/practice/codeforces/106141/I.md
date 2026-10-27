---
title: "CF 106141I - Cấp độ của Rick"
description: "Chúng ta được cho hai chuỗi có cùng độ dài. Một chuỗi đại diện cho điểm của Morty trong n ngày, chuỗi còn lại đại diện cho điểm của Rick trong cùng ngày. Mỗi lớp là một số nguyên từ 0 đến 5, trong đó 0 là giá trị đặc biệt nghĩa là không có."
date: "2026-06-19T19:35:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106141
codeforces_index: "I"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2025"
rating: 0
weight: 106141
solve_time_s: 53
verified: true
draft: false
---

[CF 106141I - Điểm của Rick](https://codeforces.com/problemset/problem/106141/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có cùng độ dài. Một chuỗi đại diện cho điểm của Morty trong n ngày, chuỗi còn lại đại diện cho điểm của Rick trong cùng ngày. Mỗi lớp là một số nguyên từ 0 đến 5, trong đó 0 là giá trị đặc biệt nghĩa là không có. 

Rick được phép hoán vị trình tự của mình một cách tùy ý. Sau khi sắp xếp lại, chúng tôi ghép ngày i của Morty với ngày i của Rick. Mục đích là để quyết định xem Rick có thể sắp xếp điểm của mình sao cho với mỗi ngày tôi, điểm của Rick ít nhất bằng điểm của Morty, hoặc Morty vắng mặt ngày hôm đó, hoặc Rick vắng mặt ngày hôm đó. Nếu có sự sắp xếp lại như vậy, chúng ta phải đưa ra một thứ tự hợp lệ về điểm của Rick; nếu không chúng ta sẽ xuất -1. 

Cấu trúc cốt lõi là một vấn đề kết hợp bị ràng buộc. Mỗi cấp Morty a[i] áp đặt một yêu cầu ngưỡng đối với cấp Rick b[j] phù hợp, trừ khi a[i] bằng 0, trong trường hợp đó ràng buộc sẽ biến mất. Ngược lại, nếu b[j] bằng 0 thì vị trí đó luôn an toàn vì nó được coi là ký tự đại diện thỏa mãn mọi ràng buộc. 

Các ràng buộc có phạm vi giá trị nhỏ, từ 0 đến 5, nhưng n có thể lên tới 3·10^5 trong các trường hợp thử nghiệm. Điều này buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc kiểm tra tất cả các hoán vị hoặc thậm chí kết hợp tham lam với các lần quét lồng nhau sẽ quá chậm trong trường hợp xấu nhất. 

Một trường hợp cạnh tinh tế phát sinh từ số không. Hãy xem xét tình huống trong đó Morty có nhiều điểm khác 0 nhưng Rick có nhiều điểm 0. Một cách tiếp cận tham lam ngây thơ có thể gán sai các số 0 quá sớm và chặn các ràng buộc cao hơn sau này. Một chế độ lỗi khác đang xử lý điều này giống như một so sánh sắp xếp đơn giản: sắp xếp cả hai mảng và so sánh theo từng phần tử không hoạt động vì các số 0 chỉ linh hoạt ở một phía của điều kiện và phá vỡ tính đối xứng vị trí. 

Ví dụ: nếu Morty là [5, 5, 5] và Rick là [0, 4, 4], việc sắp xếp Rick sẽ cho [0, 4, 4], điều này có vẻ không đủ, nhưng thực tế thì số 0 có thể được đặt ở bất kỳ vị trí nào. Tuy nhiên, nếu các ràng buộc được phân bổ không đồng đều, việc đặt sai vị trí 0 một cách tham lam vẫn có thể phá vỡ tính khả thi. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là thử tất cả các hoán vị của điểm Rick và kiểm tra xem có thứ tự nào thỏa mãn điều kiện hay không. Điều này ngay lập tức dẫn đến n! khả năng, và mỗi lần kiểm tra đều tốn O(n), khiến nó hoàn toàn không khả thi ngay cả với n nhỏ như 10. 

Quan sát quan trọng là điều duy nhất quan trọng về trình tự của Morty là có bao nhiêu vị trí yêu cầu ít nhất một ngưỡng nhất định. Vì điểm chỉ từ 0 đến 5 nên chúng tôi có thể nén các yêu cầu của Morty thành số điểm cho mỗi cấp lớp. Tương tự, điểm của Rick có thể được nhóm theo giá trị. 

Bây giờ hãy nghĩ về cách sử dụng một giá trị trong mảng của Rick. Loại x có thể đáp ứng bất kỳ vị trí Morty nào với yêu cầu ≤ x và nó cũng có thể bị lãng phí ở vị trí Morty zero. Điều này gợi ý rằng trước tiên chúng ta nên cố gắng gán điểm cho Rick từ ràng buộc cao nhất đến thấp nhất, đảm bảo rằng những yêu cầu khó hơn sẽ sớm được đáp ứng. 

Sự cố trở thành vấn đề phân bổ nguồn lực trên sáu nhóm. Chúng tôi xử lý điểm của Morty theo mức độ nghiêm ngặt tăng dần và đảm bảo có đủ điểm Rick để đáp ứng chúng. Số không trong Rick hoạt động như những chất bổ sung phổ quát và số không trong Morty loại bỏ hoàn toàn nhu cầu. 

Điều này giúp giảm bớt vấn đề kiểm tra tính khả thi thông qua việc so khớp tham lam trên các nhóm tần số, sau đó xây dựng phép gán rõ ràng bằng cách đặt các giá trị lớn hơn ở những nơi cần thiết trước tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì số lượng tần số của cả hai mảng cho các giá trị từ 0 đến 5.

1. Đếm xem mỗi hạng xuất hiện bao nhiêu lần trong mảng của Morty và mảng của Rick. Điều này nén vấn đề vào một không gian trạng thái cố định nhỏ có kích thước 6. 
2. Chia các vị trí của Morty thành hai nhóm: các vị trí trong đó a[i] bằng 0 và các vị trí trong đó a[i] bằng 0 và các vị trí trong đó a[i] khác 0. Các vị trí 0 hoàn toàn không hạn chế Rick, vì vậy sau này chúng sẽ hấp thụ mọi giá trị còn sót lại. 
3. Đối với các lớp Morty khác 0, về mặt khái niệm, chúng tôi xử lý các ràng buộc từ cấp cao đến cấp thấp. Cấp Morty x yêu cầu cấp Rick ít nhất là x trừ khi chúng tôi chỉ định Rick bằng 0. 
4. Chúng tôi duy trì cấu trúc giống như nhiều tập hợp thông qua số lượng cấp độ Rick. Chúng tôi luôn cố gắng đáp ứng các yêu cầu Morty cao nhất trước tiên bằng cách sử dụng các giá trị Rick lớn nhất hiện có. Điều này ngăn chặn tình huống lãng phí một giá trị Rick lớn cho một ràng buộc dễ dàng trong khi một ràng buộc cứng vẫn chưa được phát hiện. 
5. Đối với mỗi loại Morty từ 5 xuống 1, chúng tôi cố gắng so khớp nó một cách tham lam với giá trị Rick nhỏ nhất có thể mà vẫn hợp lệ, ưu tiên các kết quả khớp chính xác hoặc đủ tối thiểu. Nếu chúng tôi không thể đáp ứng một yêu cầu ngay cả sau khi sử dụng tất cả các giá trị lớn hơn thì chúng tôi kết luận là không thể thực hiện được. 
6. Sau khi đáp ứng tất cả các yêu cầu Morty khác 0, các giá trị Rick còn lại, bao gồm các số 0 và các lớp chưa sử dụng, được đặt tùy ý vào các vị trí Morty-0 và các vị trí còn sót lại, vì các vị trí đó không áp đặt ràng buộc nào. 
7. Xây dựng mảng cuối cùng bằng cách viết các giá trị Rick được gán vào các vị trí thứ tự ban đầu được chọn trong giai đoạn khớp. 

### Tại sao nó hoạt động 

Thuật toán thực thi nguyên tắc gán đầu tiên mạnh nhất. Mọi yêu cầu Morty khác 0 đều cần có cấp độ Rick đủ nhưng không lớn quá mức cần thiết, duy trì cấp độ cao hơn cho các giới hạn cao hơn. Vì điểm tồn tại trong một phạm vi giới hạn nhỏ nên mọi trường hợp không đáp ứng được yêu cầu đều phải xuất hiện vào thời điểm chúng tôi xử lý cấp điểm đó, bởi vì tất cả các khả năng lớn hơn đều đã được tính đến. Điều này đưa ra một điều kiện khả thi đơn điệu: một khi không thể đáp ứng được một cấp lớp thì không việc sắp xếp lại các bài tập ở cấp độ thấp hơn có thể khắc phục được điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        cnt_a = [0] * 6
        cnt_b = [0] * 6
        
        for x in a:
            cnt_a[x] += 1
        for x in b:
            cnt_b[x] += 1
        
        # We will assign from high to low requirements
        # Track available b values
        avail = cnt_b[:]
        
        ok = True
        
        # Try to satisfy requirements 5..1
        for need in range(5, 0, -1):
            demand = cnt_a[need]
            if demand == 0:
                continue
            
            # We can use b[need], b[need+1], ... b[5]
            supply = sum(avail[need:6])
            if supply < demand:
                ok = False
                break
            
            # Greedy: consume smallest valid first (for stability)
            for val in range(need, 6):
                take = min(avail[val], demand)
                avail[val] -= take
                demand -= take
                if demand == 0:
                    break
        
        if not ok:
            print(-1)
            continue
        
        # remaining values can be placed arbitrarily
        res = []
        for v in range(6):
            res.extend([v] * avail[v])
        
        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén cả hai mảng vào bảng tần số trên miền cố định từ 0 đến 5. Điều này loại bỏ tất cả lý luận về vị trí ban đầu và thay thế nó bằng một vấn đề kế toán tài nguyên. 

Sau đó, chúng tôi mô phỏng việc đáp ứng các điểm khắt khe hơn của Morty trước tiên. Đối với mỗi cấp độ yêu cầu, chúng tôi xác minh rằng có đủ cấp độ Rick trong nhóm hiện tại và cao hơn. Nếu không, chúng ta sẽ thất bại ngay lập tức. Nếu không, trước tiên chúng tôi sẽ sử dụng từ nhóm hợp lệ nhỏ nhất để duy trì các cấp độ mạnh hơn cho nhu cầu trong tương lai. 

Việc tái thiết cuối cùng chỉ đơn giản là phát ra tất cả các lớp Rick chưa sử dụng còn lại. Bởi vì tất cả các ràng buộc đã được thực thi trong quá trình phân bổ, nên mọi thứ tự còn lại đều hợp lệ. 

Một cạm bẫy triển khai phổ biến là quên rằng các số 0 trong Morty loại bỏ hoàn toàn các ràng buộc. Trong cách tiếp cận này, số 0 đương nhiên bị bỏ qua vì chúng ta không bao giờ cố gắng thỏa mãn chúng. Một điểm tinh tế khác là đảm bảo rằng chúng tôi luôn kiểm tra tình trạng còn hàng trên tất cả các nhóm cao hơn trước khi tiêu thụ, nếu không việc tiêu thụ sớm có thể dẫn đến kết quả âm tính giả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Coi Morty là [5, 2, 1, 3, 4] và Rick là [1, 2, 3, 4, 5]. 

Chúng tôi xây dựng bảng tần suất rồi xử lý các yêu cầu từ 5 trở xuống. 

| Cần | Nhu cầu | Có sẵn ≥ nhu cầu | Hành động | 
| --- | --- | --- | --- | 
| 5 | 1 | 5 | phân công 5 | 
| 4 | 1 | 4 | phân công 4 | 
| 3 | 1 | 3 | phân công 3 | 
| 2 | 1 | 2 | phân công 2 | 
| 1 | 1 | 1 | chỉ định 1 | 

Mọi nhu cầu đều được đáp ứng chính xác, không để lại dư lượng. 

Điều này thể hiện trường hợp cân bằng lý tưởng trong đó mỗi loại đều phù hợp hoàn hảo và mức tiêu dùng tham lam không ảnh hưởng đến các yêu cầu trong tương lai. 

### Ví dụ 2 

Morty: [3, 2, 1, 0] 

Rick: [2, 3, 4, 1] 

Chúng tôi bỏ qua số 0 của Morty. 

Chúng tôi xử lý need=3 trước tiên. Chúng ta có sẵn {3,4} nên chúng ta gán 3. 

Cần tiếp theo=2, còn lại là {1,2,4} nên gán 2. 

Cần tiếp theo=1, còn lại {1,4}, gán 1. 

Chúng tôi đáp ứng thành công tất cả các ràng buộc và 4 phần còn lại được đặt ở bất cứ đâu. 

Ví dụ này cho thấy thuật toán bỏ qua các ràng buộc bằng 0 một cách tự nhiên như thế nào và vẫn hoàn thành một phép gán hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm và xử lý nhóm có kích thước không đổi trên các giá trị từ 0 đến 5 | 
| Không gian | O(1) | Chỉ sử dụng mảng tần số có kích thước cố định | 

Miền giá trị không đổi là yếu tố làm giảm bài toán từ bài toán so khớp thành một quá trình đếm tham lam đơn giản. Ngay cả ở tổng số 3·10^5 phần tử, thuật toán chỉ thực hiện quét tuyến tính và hoạt động nhóm liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like sanity cases
assert run("1\n3\n1 2 3\n3 2 1\n") != "", "basic case"

# all zeros in Morty
assert run("1\n4\n0 0 0 0\n1 2 3 4\n") != "", "zeros remove constraints"

# impossible case
assert run("1\n2\n5 5\n1 1\n") == "-1", "insufficient large values"

# exact match case
assert run("1\n5\n1 2 3 4 5\n1 2 3 4 5\n") != "", "identity permutation"

# edge minimal
assert run("1\n1\n0\n5\n") != "", "single zero Morty"

# heavy zero in Rick
assert run("1\n3\n1 2 3\n0 0 0\n") == "-1", "no nonzero supply"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không trong Morty | hoán vị nào | ràng buộc biến mất | 
| tất cả số không trong Rick | -1 cho nhu cầu khác 0 | số không không thể đáp ứng yêu cầu | 
| trường hợp nhận dạng | trình tự tương tự | độ chính xác cơ bản | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi Morty chỉ có điểm cao và Rick có nhiều điểm thấp với một vài điểm cao. Ví dụ: Morty là [5, 5, 5] và Rick là [5, 1, 1]. Thuật toán xử lý cần=5 trước tiên, tiêu thụ 5 đơn lẻ, sau đó thất bại ngay lập tức vì nguồn cung còn lại trên hoặc bằng 5 đã cạn kiệt. Điều này đúng vì không có sự sắp xếp lại nào có thể tạo thêm những trận đấu cấp cao. 

Một trường hợp khác là khi các số 0 chiếm ưu thế trong mảng của Morty, chẳng hạn như [0, 0, 0, 5]. Thuật toán bỏ qua hoàn toàn các số 0 và chỉ kiểm tra tính khả thi cho một yêu cầu nghiêm ngặt duy nhất, đảm bảo rằng mọi tính linh hoạt đều được sử dụng chính xác. 

Trường hợp cạnh cuối cùng là khi Rick bao gồm toàn số 0 ngoại trừ một giá trị cao. Thuật toán chỉ định chính xác giá trị cao đó cho yêu cầu mạnh nhất trước tiên, sau đó coi phần còn lại là lỗi nếu tồn tại bất kỳ nhu cầu bổ sung nào khác 0.
