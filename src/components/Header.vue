<template>
    <nav class="navbar navbar-expand-lg bg-light">
        <div class="container-fluid">
         
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item">
                <a class="nav-link" href="#">Questions {{numCorrect}} / {{numTotal}}</a>
              </li>         
            </ul>
          </div>
        </div>
      </nav>
</template>
<script>

export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>
