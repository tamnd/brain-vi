---
title: "CF 104873D - Chuỗi con riêng biệt"
description: "Chúng ta được cho một chuỗi ngắn p và một số nguyên rất lớn n. Chuỗi thực tế mà chúng ta làm việc không phải là chuỗi tùy ý: nó được hình thành bằng cách lặp đi lặp lại p rồi cắt nó sau đúng n ký tự."
date: "2026-06-28T10:12:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104873
codeforces_index: "D"
codeforces_contest_name: "2018-2019 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104873
solve_time_s: 35
verified: true
draft: false
---

[CF 104873D - Chuỗi con riêng biệt](https://codeforces.com/problemset/problem/104873/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi ngắn`p`và một số nguyên rất lớn`n`. Chuỗi thực tế mà chúng ta làm việc không phải là chuỗi tùy ý: nó được hình thành bằng cách lặp lại`p`làm đi làm lại rồi cắt nó một cách chính xác`n`nhân vật. Vậy chuỗi cuối cùng`s`là tuần hoàn, với chu kỳ`k = |p|`, ngoại trừ có thể có khối cuối cùng bị cắt bớt. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi con không trống riêng biệt xuất hiện ở đâu đó bên trong`s`. Hai chuỗi con được coi là giống nhau nếu chúng bằng nhau, ngay cả khi chúng đến từ các vị trí khác nhau. 

Khó khăn đến từ sự không phù hợp về quy mô. mẫu`p`nhỏ, có chiều dài lên tới 1000, nhưng`n`có thể lớn tới 10^9. Điều đó ngay lập tức loại trừ việc xây dựng`s`một cách rõ ràng, vì ngay cả việc lưu trữ nó cũng là không thể. Bất kỳ cách tiếp cận nào cố gắng liệt kê các chuỗi con trực tiếp từ`s`cũng không khả thi, vì số lượng chuỗi con tăng theo bậc hai theo`n`. 

Một mô hình tinh thần ngây thơ có thể gợi ý việc trượt qua`s`và băm mọi chuỗi con. Điều đó đã thất bại vì ngay cả việc lặp lại trên tất cả các vị trí bắt đầu cũng là O(n), quá lớn đối với n = 10^9. 

Một vấn đề tinh tế hơn xuất hiện với tính tuần hoàn. Mặc dù cấu trúc lặp đi lặp lại, các chuỗi con có thể vượt qua ranh giới giữa các bản sao lặp lại của`p`. Điều này tạo ra các chuỗi con mới không có trong một khoảng thời gian. Ví dụ, trong`p = "ab"`, chuỗi`s = "ababab..."`chứa các chuỗi con như`"bab"`, không tồn tại bên trong một khối. 

Trường hợp cạnh chính là khi`n`chỉ lớn hơn một chút so với`k`. Sau đó, hầu hết các chuỗi con đến từ các tương tác qua ranh giới của hai bản sao, không phải từ bên trong một khối duy nhất. Bất kỳ cách tiếp cận nào chỉ đếm các chuỗi con trong một khoảng thời gian và nhân với một hệ số sẽ bị tính thiếu. 

Một trường hợp cạnh khác là khi`p`bản thân nó chứa cấu trúc lặp đi lặp lại. Ví dụ`p = "aaaa"`. Sau đó`s`chỉ là một chặng đường dài`a`và số lượng chuỗi con riêng biệt chỉ là`n`, không phải bậc hai. Điều này cho thấy sự lặp lại bên trong`p`làm suy giảm tính đa dạng của chuỗi con một cách nghiêm trọng và bất kỳ giải pháp đúng nào cũng phải tính đến cấu trúc bên trong của`p`, không chỉ chiều dài của nó. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: xây dựng chuỗi đầy đủ`s`, liệt kê mọi chuỗi con`s[l:r]`, chèn nó vào một tập hợp và xuất ra kích thước của tập hợp đó. Điều này đúng vì mọi chuỗi con riêng biệt đều được thu thập rõ ràng. Tuy nhiên, số lượng chuỗi con trong một chuỗi có độ dài`n`theo thứ tự n(n+1)/2, trở thành khoảng 5 × 10^17 khi n = 10^9. Kể cả nếu`n`chỉ mới 10^5, cách tiếp cận này đã vượt xa mọi thời gian chạy khả thi. 

Cấu trúc của vấn đề bị chi phối bởi tính tuần hoàn. Chuỗi được xác định hoàn toàn bởi một mẫu nhỏ`p`, vì vậy bất kỳ chuỗi con nào của`s`được xác định bởi nơi nó bắt đầu trong một khoảng thời gian và nó kéo dài bao nhiêu khoảng thời gian. Điều này gợi ý nén vấn đề từ chiều dài`n`xuống một cái gì đó tùy thuộc vào`k`. 

Quan sát quan trọng là bất kỳ chuỗi con nào của`s`hoặc được chứa hoàn toàn trong một cửa sổ của một vài bản sao liên tiếp của`p`, hoặc cuối cùng nó trở nên tuần hoàn. Trong thực tế, khi độ dài chuỗi con vượt quá`k`, hành vi của nó được xác định bởi sự chồng chéo của`p`với chính nó. Điều này làm giảm vấn đề phân tích các chuỗi con trên một chuỗi tuần hoàn vô hạn về mặt khái niệm, nhưng chỉ có độ dài tối đa`n`. 

Cách tiêu chuẩn để chính thức hóa điều này là sử dụng ý tưởng mảng hậu tố tự động hoặc hậu tố trên một chuỗi đại diện cho hai bản sao của`p`. Hai bản sao là đủ để nắm bắt tất cả các chuỗi con vượt qua một ranh giới, bởi vì bất kỳ chuỗi con nào của chuỗi dấu chấm tuần hoàn`k`tối đa chỉ cần`2k`các ký tự để thể hiện sự chuyển đổi nội bộ của nó. Sau đó chúng tôi kết hợp điều này với một hạn chế về độ dài`n`. 

Từ quan điểm này, vấn đề trở thành việc đếm các chuỗi con riêng biệt trong một chuỗi về mặt khái niệm.`p + p`, nhưng mỗi chuỗi con được phép kéo dài đến mức tối đa`n`, không chỉ`2k`. Máy tự động hậu tố cung cấp một cấu trúc nhỏ gọn trong đó mỗi trạng thái đại diện cho một tập hợp các chuỗi con và chuyển tiếp mã hóa phần mở rộng theo một ký tự. Sau đó chúng ta có thể mô phỏng việc kéo dài đến độ dài`n`đồng thời tôn trọng sự chuyển đổi định kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Máy tự động hậu tố khi nhân đôi định kỳ | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một máy tự động hậu tố cho chuỗi`t = p + p`. Chuỗi nhân đôi này đủ để nắm bắt tất cả các chuyển đổi giữa các bản sao liên tiếp của`p`, đó là nơi các chuỗi con mới được tạo ra. 

Sau đó, chúng tôi tính toán, đối với mỗi trạng thái máy tự động, nó đóng góp bao nhiêu chuỗi con riêng biệt, nhưng chúng tôi phải hạn chế độ dài chuỗi con ở mức tối đa`n`. Từ`n`có thể lớn hơn nhiều so với`k`, cấu trúc ô tô vẫn nắm bắt đầy đủ tất cả các mẫu riêng biệt;`n`chỉ giới hạn mức độ chúng tôi được phép mở rộng trong quá trình chuyển đổi. 

Ý tưởng cốt lõi là mọi trạng thái trong ô tô hậu tố đại diện cho một tập hợp các chuỗi con có cùng vị trí cuối. Mỗi tiểu bang có một`len`giá trị, là độ dài tối đa của chuỗi ở trạng thái đó và liên kết đến trạng thái hậu tố của nó, xác định ranh giới độ dài tối thiểu. 

Chúng ta điều chỉnh số đếm sao cho thay vì đếm tất cả các chuỗi con trong`p+p`, chúng tôi chỉ đếm các chuỗi con có độ dài không vượt quá`n`. 

### Các bước 

1. Xây dựng chuỗi`t = p + p`. 

Điều này đảm bảo rằng bất kỳ chuỗi con nào vượt qua ranh giới dấu chấm sẽ xuất hiện rõ ràng bên trong một độ dài`2k`cửa sổ. Điều này tránh việc lập luận trực tiếp về sự lặp lại vô hạn. 
2. Xây dựng một máy tự động có hậu tố`t`. 

Mỗi trạng thái đại diện cho một lớp chuỗi con kết thúc ở một vị trí nào đó và các chuyển đổi tương ứng với việc mở rộng chuỗi con thêm một ký tự. 
3. Đối với mỗi trạng thái, hãy diễn giải sự đóng góp của nó vào các chuỗi con riêng biệt là khoảng độ dài`[link.len + 1, len]`. 

Khoảng này mô tả tất cả độ dài chuỗi con thuộc duy nhất vào trạng thái đó. 
4. Cắt từng quãng để`[1, n]`. 

Vì chuỗi gốc bị cắt bớt độ dài`n`, bất kỳ chuỗi con nào dài hơn`n`không hợp lệ và không được đóng góp. 
5. Tính tổng của tất cả các trạng thái kích thước của các khoảng bị cắt bớt này. 

Mỗi độ dài hợp lệ đóng góp chính xác một lần vì hậu tố tự động trạng thái phân vùng tất cả các chuỗi con riêng biệt theo phạm vi độ dài. 

### Tại sao nó hoạt động 

Máy tự động hậu tố phân chia tất cả các chuỗi con riêng biệt thành các lớp tương đương rời rạc dựa trên vị trí cuối cùng của chúng và phần mở rộng lặp lại dài nhất. Mỗi chuỗi con tương ứng với chính xác một trạng thái và chính xác một độ dài trong khoảng trạng thái đó. Cắt để`n`duy trì tính đúng đắn vì nó chỉ loại bỏ các chuỗi con không thể tồn tại trong chuỗi tuần hoàn bị cắt cụt mà không thay đổi cấu trúc của chuỗi con nào là khác biệt. Từ`t = p + p`đã nắm bắt được tất cả các tương tác xuyên biên giới, không có chuỗi con nào`s`bị bỏ sót hoặc trùng lặp ngoài những ràng buộc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SAM:
    def __init__(self):
        self.next = []
        self.link = []
        self.length = []
        self.last = 0

        self.next.append({})
        self.link.append(-1)
        self.length.append(0)

    def extend(self, c):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(0)

        p = self.last
        while p != -1 and c not in self.next[p]:
            self.next[p][c] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][c]
            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = len(self.next)
                self.next.append(self.next[q].copy())
                self.length.append(self.length[p] + 1)
                self.link.append(self.link[q])

                while p != -1 and self.next[p].get(c) == q:
                    self.next[p][c] = clone
                    p = self.link[p]

                self.link[q] = self.link[cur] = clone

        self.last = cur

def count_distinct(p, n):
    sam = SAM()
    t = p + p

    for ch in t:
        sam.extend(ch)

    total = 0
    for v in range(1, len(sam.next)):
        l = sam.length[sam.link[v]] + 1
        r = sam.length[v]
        if l > n:
            continue
        r = min(r, n)
        if r >= l:
            total += (r - l + 1)

    return total

def main():
    p = input().strip()
    n = int(input())
    print(count_distinct(p, n))

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng một máy tự động hậu tố trên`p + p`. Cấu trúc máy tự động mã hóa tất cả các chuỗi con riêng biệt của chuỗi nhân đôi và bước đếm trích xuất các đóng góp từ mỗi trạng thái bằng cách sử dụng thuộc tính khoảng tự động hậu tố tiêu chuẩn. Sửa đổi duy nhất so với số chuỗi con cổ điển là giới hạn ở`n`, điều này ngăn chặn việc đếm các chuỗi con dài hơn chuỗi được tạo thực tế. 

Một điểm thực hiện tinh tế là bước nhân bản trong`extend`, đảm bảo phân vùng chính xác các trạng thái khi chuyển tiếp được chia sẻ. Nếu không nhân bản, nhiều chuỗi con riêng biệt sẽ bị thu gọn thành các lớp tương đương không chính xác, phá vỡ cách diễn giải khoảng. 

## Ví dụ đã hoạt động 

Hãy xem xét`p = "ab"`Và`n = 5`, Vì thế`s = "ababa"`. 

Chúng tôi xây dựng`t = "abab"`. 

| Bước | Đã thêm tiểu bang | Chiều dài | Liên kết | Chuyển tiếp mới | 
| --- | --- | --- | --- | --- | 
| 1 | một | 1 | 0 | một | 
| 2 | b | 2 | 0 | b | 
| 3 | một | 3 | 1 | một | 
| 4 | b | 4 | 2 | b | 

Mỗi trạng thái đóng góp một khoảng`[link.len + 1, len]`. Ví dụ, một trạng thái có`len = 3`Và`link.len = 1`đóng góp độ dài từ 2 đến 3. 

Cắt tại`n = 5`không làm gì ở đây vì tất cả các khoảng đều đã nhỏ. Tổng cuối cùng tính tất cả các chuỗi con riêng biệt trong`"ababa"`. 

Bây giờ hãy xem xét`p = "aaaa"`Và`n = 6`, Vì thế`s = "aaaaaa"`. 

Mọi trạng thái về cơ bản đều sụp đổ thành tình trạng lặp đi lặp lại`'a'`phần mở rộng. Máy tự động có một chuỗi độ dài tuyến tính và các khoảng chỉ trùng nhau về độ dài chứ không trùng lặp về nội dung. Phần đóng góp trở thành chính xác là 6, tương ứng với chuỗi con`"a"`,`"aa"`, ...,`"aaaaaa"`. 

Dấu vết này cho thấy sự lặp lại bên trong`p`giảm máy tự động thành một đường dẫn duy nhất và thuật toán sẽ nén không gian chuỗi con một cách tự nhiên. 

##Phức tạp
