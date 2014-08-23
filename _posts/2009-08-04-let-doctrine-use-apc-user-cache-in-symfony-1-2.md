---
layout: post
title: !binary |-
  TGV0IERvY3RyaW5lIHVzZSBBUEMgVXNlciBDYWNoZSBpbiBTeW1mb255IDEu
  Mg==
created: 1249391684
comments: true
categories: !binary |-
  ZW5fdXMgc29mdHdhcmU=
---
Simply drop a new function of some lines inside ProjectConfiguration.class.php

<pre lang="PHP">
  public function configureDoctrine(Doctrine_Manager $manager)
  {
    if (ini_get("apc.enabled")==1)
      if (sfApplicationConfiguration::getEnvironment() != "dev")
        $manager->setAttribute(Doctrine::ATTR_QUERY_CACHE, new Doctrine_Cache_Apc());
      else apc_clear_cache("user");
  }
</pre>

Every result will be cached inside the apc user cache, so the load on database will be reduced.
